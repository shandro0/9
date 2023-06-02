<h1 align="center"> МИНИСТЕРСТВО НАУКИ И ВЫСШЕГО ОБРАЗОВАНИЯ РОССИЙСКОЙ ФЕДЕРАЦИИ ФЕДЕРАЛЬНОЕ ГОСУДАРСТВЕННОЕ БЮДЖЕТНОЕ ОБРАЗОВАТЕЛЬНОЕ УЧРЕЖДЕНИЕ ВЫСШЕГО ОБРАЗОВАНИЯ «САХАЛИНСКИЙ ГОСУДАРСТВЕННЫЙ УНИВЕРСИТЕТ»</h1>






<p align="center">Лабораторная работа №9 <br> "JS" </p>






<p align="right">Выполнил: Шандро Н.Б.</p>
<p align="right">Проверил: Соболев Е. И.</p>







<p align="center">г. Южно-Сахалинск <br> 2023 год</p>




<h2 align="center">Введение</h2>
<p align="justify">Решение задач на JavaScript</p>

<h2 align="center">Цели и задачи</h2>

1. Есть некоторая строка `(var str = 'fgfggg';)`, что будет, если мы возьмем `str[0]`?
2. Дана функция, она принимает в качестве аргументов строки '*', '1', 'b', '1c', реализуйте ее так, что бы она вернула строку '1*b*1c'
3. Дано дерево, надо найти сумму всех вершин.
4. Нарисовать стилями полукруг.
5. Есть массив в котором лежат объекты с датами, отсортировать по датам.
6. Есть несколько слов, определить состоят ли они из одних и тех же букв('кот', 'ток', 'окт')
7. От них же. Числа от 1 до 100 лежат в массиве, они хаотично перемешанные, от туда изъяли одно число, надо найти, что это за число. алгоритм не должен превышать O(n^2) сложности.
8. Реализовать сортировку пузырьком.
9. Обратная польская нотация.
10. Реализовать функцию является ли слово палендром.


<h2 align="center">Решение задач</h2>


1. 
```javascript
function zad1() {
    var str = 'fgfggg';
    insertInPage("insert-1", str[0]);
}
```
2. 
```javascript
function zad2(action, ...args) {
    var str = '';
    for(let i = 0; i < args.length; i++){
        str+=args[i];
        if(i+1 != args.length)
            str+=action;
    }

    insertInPage("insert-2", str);
}
```
3. 
```javascript
class Node {
    static count = 0;

    ID;
    value;
    parent;
    children;

    constructor(value, parent) {
        this.ID = Node.count.toString();
        this.value = value;
        this.parent = parent;
        Node.count++;
    }
}

const tree = {
    head: new Node(undefined, null),
    Randomise(depth, childrenRange) {
        this.head.value = Math.floor(Math.random() * 10);

        if (depth > 0) {
            this.addChildrenTo(this.head, Math.floor(Math.random() * childrenRange) + 1);

            if (depth > 1) {
                let queue = [...this.head.children];
                depth--;

                while (depth > 0) {
                    let temp = [];
                    for (let i in queue) {
                        this.addChildrenTo(queue[i], Math.floor(Math.random() * childrenRange) + 1);
                        temp.push(queue[i].children);
                    }
                    queue = temp;
                    depth--;
                }
            }
        }
    },
    addChildrenTo(node, count) {
        node.children = [];
        for (let i = 0; i < count; i++) {
            node.children.push(new Node(Math.floor(Math.random() * 10), node));
        }
    }
}

function zad3(){
    tree.Randomise(depth = 2, childrenRange = 3);
    appendInPage("insert-3","Дерево: "+ tree);
    appendInPage("insert-3","Всего вершин: " + Node.count);

    var sum = SumTree(tree.head);
    appendInPage("insert-3","Сумма вершин: " + sum);
}

function SumTree(node) {
    let sum = node.value;

    if (node.children == undefined) {
        console.log("id: ", node.ID, "=", node.value);
        return node.value;
    }

    node.children.forEach(child => {
        sum += SumTree(child);
    });

    console.log("id: ", node.ID, "=", node.value);
    return sum;
}
```
4. 
```javascript
function zad4(){
    var canvas = document.getElementById("insert-4");
    var ctx = canvas.getContext("2d");
    var centerX = canvas.width / 2;
    var centerY = canvas.height / 2;
    var radius = 70;
    ctx.beginPath();
    ctx.arc(centerX, centerY, radius, 0, Math.PI, true);
    ctx.stroke();
}
```
5. 
```javascript
function zad5(){
    var array = [
        {id: 1, date:'Mar 25 2013 10:27:00 AM'},
        {id: 2, date:'Nov 13 2022 08:42:00 AM'},
        {id: 3, date:'Apr 6 2021 10:26:00 AM'},
        {id: 4, date:'Dec 2 2022 10:00:00 AM'},
        {id: 5, date:'Feb 19 2022 05:00:00 PM'},
        {id: 6, date:'Jan 31 2021 10:38:00 AM'},
        {id: 7, date:'Aug 1 2022 10:59:00 AM'},
        {id: 8, date:'Aug 1 2022 11:50:00 AM'}
    ];

    array.sort(function(a,b){
        var c = new Date(a.date);
        var d = new Date(b.date);
        return c-d;
    });

    let output = "";

    for(let i = 0; i < array.length; i++){
        output+="id: "+array[i].id+"  date: "+array[i].date+"<br>";
    }
    insertInPage("insert-5", output);
}
```
6. 
```javascript
function zad6(...args){
    var words = [];
    for(let i = 0; i < args[0].length; i++)
        words[i] = args[0][i];
    
    for(let i = 1; i < args.length; i++){
        for(let j = 0; j < args[i].length; j++){
            let symbol = args[i][j];
            if(words.indexOf(symbol) === -1){
                insertInPage("insert-6", "<p>Слова " + args + " НЕ состоят из одних и техже букв</p>");
                return;
            }
        }
    }
    insertInPage("insert-6", "<p>Слова " + args + " состоят из одних и техже букв</p>");
}

function GetRandomArray(){
    var arr = [];
    while(arr.length < 100){
        var r = Math.floor(Math.random() * 100) + 1;
        if(arr.indexOf(r) === -1) arr.push(r);
    }
    return arr;
}
```
7. 
```javascript
function zad7(){  
    var nums = GetRandomArray();
    var r = Math.floor(Math.random() * 100) + 1;

    // Удаляем рандомный элемент
    nums.splice(r, 1);

    //Поиск пропавшего числа
    var loseNum = 0;
    nums.sort(function(a, b){return a-b}); 

    for(let i = 0; i < nums.length-1; i++) {
        if(nums[i+1]-nums[i] > 1){
            loseNum = nums[i]+1;
            break;
        }
    }  

    insertInPage("insert-7", loseNum);
}
```
8. 
```javascript
function zad8(){
    var str = "";

    var nums = GetRandomArray();
    str+=nums+"<br>";

    var sortNums = bubbleSort(nums);
    str+=sortNums+"<br>";

    insertInPage("insert-8", str);
}

function bubbleSort(nums){
    for(let i = 0; i < nums.length; i++) {
        for(let j = 0; j < nums.length; j++) {
            if (nums[j] > nums[j + 1]) {
                let tmp = nums[j];
                nums[j] = nums[j + 1];
                nums[j + 1] = tmp;
            }
        }
    }
    return nums;
}
```
9. 
```javascript
function zad9(){
    let statement = '1 + 2 * 3 / (4+5) * (6+7)';

	let result = '';
    let stack = [];
    
    let numbers = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'];
    const operators = { '+': 0, '-': 0, '*': 1, '/': 1, '^': 2 };
    
    for (let i = 0; i < statement.length; ++i) {
        const c = statement.charAt(i);
        
        if (numbers.indexOf(c) >= 0 ) {
            result += c+" ";
        } 
        else if (c === '(') {
            stack.push(c);
        } 
        else if (c === ')') {
            let s = stack.pop();
        
            while (s && s != '(') {
                result += s+" ";
                s = stack.pop();
            } 
        } 
        else if (Object.keys(operators).indexOf(c) >= 0) {
            while ( operators[stack.slice(-1)[0]] >= operators[c]) {
            result += stack.pop()+" ";
        }
        
        stack.push(c);
        }
    }
    
    let sym = '';
    
    while (sym = stack.pop()) {
        result += sym+" ";
    }

    insertInPage("insert-9", result);
}
```
10. 
```javascript
function zad10(str){
    var splitString = str.split(""); 
    var reverseArray = splitString.reverse(); 
    var reverseString = reverseArray.join("");

    if(reverseString == str) insertInPage("insert-10", str+" - палиндром");
    else insertInPage("insert-10", str+" - не палиндром");
}
```

<h2 align="center">Вывод</h2>
Попрактиковался в решении задач на JavaScript;
