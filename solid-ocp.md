# S.O.L.I.D 五大原则之开闭原则 OCP 

## 前言 

本章我们要讲解的是 S.O.L.I.D 五大原则 JavaScript 语言实现的第 2 篇，开闭原则 OCP（The Open/Closed Principle ）。

开闭原则的描述是：

> Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.  
> 软件实体（类，模块，方法等等）应当对扩展开放，对修改关闭，即软件实体应当在不修改的前提下扩展。

open for extension（对扩展开放）的意思是说当新需求出现的时候，可以通过扩展现有模型达到目的。而 Close for modification（对修改关闭）的意思是说不允许对该实体做任何修改，说白了，就是这些需要执行多样行为的实体应该设计成不需要修改就可以实现各种的变化，坚持开闭原则有利于用最少的代码进行项目维护。

## 问题代码

为了直观地描述，我们来举个例子演示一下，下属代码是动态展示 question 列表的代码（没有使用开闭原则）。

```
// 问题类型
var AnswerType = {
    Choice: 0,
    Input: 1
};
// 问题实体
function question(label, answerType, choices) {
    return {
        label: label,
        answerType: answerType,
        choices: choices // 这里的choices是可选参数
    };
}
var view = (function () {
    // render一个问题
    function renderQuestion(target, question) {
        var questionWrapper = document.createElement('div');
        questionWrapper.className = 'question';
        var questionLabel = document.createElement('div');
        questionLabel.className = 'question-label';
        var label = document.createTextNode(question.label);
        questionLabel.appendChild(label);
        var answer = document.createElement('div');
        answer.className = 'question-input';
        // 根据不同的类型展示不同的代码：分别是下拉菜单和输入框两种
        if (question.answerType === AnswerType.Choice) {
            var input = document.createElement('select');
            var len = question.choices.length;
            for (var i = 0; i < len; i++) {
                var option = document.createElement('option');
                option.text = question.choices[i];
                option.value = question.choices[i];
                input.appendChild(option);
            }
        }
        else if (question.answerType === AnswerType.Input) {
            var input = document.createElement('input');
            input.type = 'text';
        }
        answer.appendChild(input);
        questionWrapper.appendChild(questionLabel);
        questionWrapper.appendChild(answer);
        target.appendChild(questionWrapper);
    }
    return {
        // 遍历所有的问题列表进行展示
        render: function (target, questions) {
            for (var i = 0; i < questions.length; i++) {
                renderQuestion(target, questions[i]);
            };
        }
    };
})();
var questions = [
                question('Have you used tobacco products within the last 30 days?', AnswerType.Choice, ['Yes', 'No']),
                question('What medications are you currently using?', AnswerType.Input)
                ];
var questionRegion = document.getElementById('questions');
view.render(questionRegion, questions);
```

上面的代码，view 对象里包含一个 render 方法用来展示 question 列表，展示的时候根据不同的 question 类型使用不同的展示方式，一个 question 包含一个 label 和一个问题类型以及 choices 的选项（如果是选择类型的话）。如果问题类型是 Choice 那就根据选项生产一个下拉菜单，如果类型是 Input，那就简单地展示 input输入框。

该代码有一个限制，就是如果再增加一个 question 类型的话，那就需要再次修改 renderQuestion 里的条件语句，这明显违反了开闭原则。

## 重构代码

让我们来重构一下这个代码，以便在出现新 question 类型的情况下允许扩展 view 对象的 render 能力，而不需要修改 view 对象内部的代码。

先来创建一个通用的 questionCreator 函数：

```
function questionCreator(spec, my) {
    var that = {};
    my = my || {};
    my.label = spec.label;
    my.renderInput = function () {
        throw "not implemented"; 
        // 这里renderInput没有实现，主要目的是让各自问题类型的实现代码去覆盖整个方法
    };
    that.render = function (target) {
        var questionWrapper = document.createElement('div');
        questionWrapper.className = 'question';
        var questionLabel = document.createElement('div');
        questionLabel.className = 'question-label';
        var label = document.createTextNode(spec.label);
        questionLabel.appendChild(label);
        var answer = my.renderInput();
        // 该render方法是同样的粗合理代码
        // 唯一的不同就是上面的一句my.renderInput()
        // 因为不同的问题类型有不同的实现
        questionWrapper.appendChild(questionLabel);
        questionWrapper.appendChild(answer);
        return questionWrapper;
    };
    return that;
}
```

该代码的作用组合要是 render 一个问题，同时提供一个未实现的 renderInput 方法以便其他 function 可以覆盖，以使用不同的问题类型，我们继续看一下每个问题类型的实现代码：

```
function choiceQuestionCreator(spec) {
    var my = {},
that = questionCreator(spec, my);           
    // choice类型的renderInput实现
    my.renderInput = function () {
        var input = document.createElement('select');
        var len = spec.choices.length;
        for (var i = 0; i < len; i++) {
            var option = document.createElement('option');
            option.text = spec.choices[i];
            option.value = spec.choices[i];
            input.appendChild(option);
        }
        return input;
    };
    return that;
}
function inputQuestionCreator(spec) {
    var my = {},
that = questionCreator(spec, my);
    // input类型的renderInput实现
    my.renderInput = function () {
        var input = document.createElement('input');
        input.type = 'text';
        return input;
    };
    return that;
}
```

choiceQuestionCreator 函数和 inputQuestionCreator 函数分别对应下拉菜单和 input 输入框的 renderInput 实现，通过内部调用统一的 questionCreator(spec, my)然后返回 that 对象（同一类型哦）。

view 对象的代码就很固定了。

```
var view = {
    render: function(target, questions) {
        for (var i = 0; i < questions.length; i++) {
            target.appendChild(questions[i].render());
        }
    }
};
```

所以我们声明问题的时候只需要这样做，就 OK 了：

```
var questions = [
    choiceQuestionCreator({
    label: 'Have you used tobacco products within the last 30 days?',
    choices: ['Yes', 'No']
　　}),
    inputQuestionCreator({
    label: 'What medications are you currently using?'
　　})
    ];
```

最终的使用代码，我们可以这样来用：

```
var questionRegion = document.getElementById('questions');
view.render(questionRegion, questions);
```

重构后的最终代码

```
function questionCreator(spec, my) {
    var that = {};
    my = my || {};
    my.label = spec.label;
    my.renderInput = function() {
        throw "not implemented";
    };
    that.render = function(target) {
        var questionWrapper = document.createElement('div');
        questionWrapper.className = 'question';
        var questionLabel = document.createElement('div');
        questionLabel.className = 'question-label';
        var label = document.createTextNode(spec.label);
        questionLabel.appendChild(label);
        var answer = my.renderInput();
        questionWrapper.appendChild(questionLabel);
        questionWrapper.appendChild(answer);
        return questionWrapper;
    };
    return that;
}
function choiceQuestionCreator(spec) {
    var my = {},
        that = questionCreator(spec, my);
    my.renderInput = function() {
        var input = document.createElement('select');
        var len = spec.choices.length;
        for (var i = 0; i < len; i++) {
            var option = document.createElement('option');
            option.text = spec.choices[i];
            option.value = spec.choices[i];
            input.appendChild(option);
        }
        return input;
    };
    return that;
}
function inputQuestionCreator(spec) {
    var my = {},
        that = questionCreator(spec, my);
    my.renderInput = function() {
        var input = document.createElement('input');
        input.type = 'text';
        return input;
    };
    return that;
}
var view = {
    render: function(target, questions) {
        for (var i = 0; i < questions.length; i++) {
            target.appendChild(questions[i].render());
        }
    }
};
var questions = [
    choiceQuestionCreator({
    label: 'Have you used tobacco products within the last 30 days?',
    choices: ['Yes', 'No']
}),
    inputQuestionCreator({
    label: 'What medications are you currently using?'
})
    ];
var questionRegion = document.getElementById('questions');
view.render(questionRegion, questions);
```

上面的代码里应用了一些技术点，我们来逐一看一下：


1. 首先，questionCreator 方法的创建，可以让我们使用模板方法模式将处理问题的功能 delegat 给针对每个问题类型的扩展代码 renderInput 上。
2. 其次，我们用一个私有的 spec 属性替换掉了前面 question 方法的构造函数属性，因为我们封装了 render 行为进行操作，不再需要把这些属性暴露给外部代码了。
3. 第三，我们为每个问题类型创建一个对象进行各自的代码实现，但每个实现里都必须包含 renderInput 方法以便覆盖 questionCreator 方法里的 renderInput 代码，这就是我们常说的策略模式。
4. 通过重构，我们可以去除不必要的问题类型的枚举 AnswerType，而且可以让 choices 作为 choiceQuestionCreator 函数的必选参数（之前的版本是一个可选参数）。

## 总结 

重构以后的版本的 view 对象可以很清晰地进行新的扩展了，为不同的问题类型扩展新的对象，然后声明 questions 集合的时候再里面指定类型就行了，view 对象本身不再修改任何改变，从而达到了开闭原则的要求。

另：懂 C#的话，不知道看了上面的代码后是否和多态的实现有些类似？其实上述的代码用原型也是可以实现的，大家可以自行研究一下。

