# Sorting-Visualizer
Sorting-Visualizer created in VSCode using HTML, CSS and JavaScript
let randomize_array = document.getElementById("randomize-array-btn");
let bubbleSort_btn = document.getElementById("bubble-sort-btn");
let mergeSort_btn = document.getElementById("merge-sort-btn");
let quickSort_btn = document.getElementById("quick-sort-btn");
let bars_container = document.getElementById("bars-container");
let minRange = 1;
let maxRange = 40;
let numOfBars = 60;
let speed = 30;
let unsortedArray = new Array(numOfBars);

function randomNum(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
} 

function createRandomArray() {
    for (let i = 0; i < numOfBars; i++) {
        unsortedArray[i] = randomNum(minRange, maxRange);
        console.log(unsortedArray[i]) 
    }
}

document.addEventListener("DOMContentLoaded", function () {
    createRandomArray();
    renderBars(unsortedArray);
});

function renderBars(array){
    for (let i = 0; i < array.length; i++) {
        let bar = document.createElement("div");
        bar.classList.add("bar");
        bar.style.height = array[i] * 12 + "px";
        bars_container.appendChild(bar);
        
    }
}

randomize_array.addEventListener("click", function () {
    createRandomArray();
    bars_container.innerHTML = "";
    renderBars(unsortedArray);
    
});

function sleep(ms){
    return new Promise((resolve) => setTimeout(resolve, ms));
}

function waitforme(milisec) { 
    return new Promise(resolve => { 
        setTimeout(() => { resolve('') }, milisec); 
    }) 
}


//----------------Bubble sort algorithm----------------------------------------->
async function bubbleSort(array){
    let speed = 15;
    let bars = document.getElementsByClassName("bar");
    for (let i = 0; i < array.length; i++) {
        for (let j = 0; j < array.length - i - 1; j++) {
            if(array[j] > array[j+1]){
                for (let k = 0; k < bars.length; k++) {
                    if(k !== j && k !== j + 1){
                        bars[k].style.backgroundColor = "aqua";
                    }
                }
                let temp = array[j];
                array[j] = array[j+1];
                array[j+1] = temp;
                bars[j].style.height = array[j] * 12 + "px";
                bars[j].style.backgroundColor = "yellow";
                //bars[j].innerText = array[j];
                bars[j+1].style.height = array[j+1] * 12 + "px";
                bars[j+1].style.backgroundColor = "orange";
                //bars[j+1].innerText = array[j+1];
                await sleep(speed);
            }
        }
        await sleep(speed);

    }
    return array;
}

bubbleSort_btn.addEventListener("click", function () {
    let sorted_array = bubbleSort(unsortedArray);
    console.log(sorted_array);
});

//--------------------------------------Quick sort algorithm-------------------------------------------->
async function swap(array, left, right){
    var temp = array[left];
    array[left] = array[right];
    array[right] = temp;
    
}

async function partition(array, left, right){
    var pivotIdx = Math.floor((right + left)/2),
    pivot = array[pivotIdx],
    i = left,
    j = right;
    let bars = document.getElementsByClassName("bar");
    let s = 200;
    bars[pivotIdx].style.backgroundColor="yellow";
    while(i <= j){
        await waitforme(s);
        while(array[i] < pivot){
            bars[i].style.backgroundColor="green";
            i ++;
        }
        while(array[j] > pivot){
            bars[j].style.backgroundColor="red";
            j --;
        }
        if(i <= j){
            await swap(array, i, j);
            bars[i].style.height = array[i]*12 + "px";
            bars[j].style.height = array[j]* 12 + "px";
            bars[i].style.backgroundColor="aqua";
            bars[j].style.backgroundColor="aqua";
            i ++;
            j --;
        }
    }
    bars[i].style.backgroundColor="aqua";
    bars[j].style.backgroundColor="aqua";
    return i;
};

async function quickSort(array, left, right){
    //let s = 1000;
    var index;
    if(array.length > 1){
        index = await partition(array, left, right);
        if(left < index - 1){
            await quickSort(array, left, index - 1);
        }
        
        if(index < right){
           
            await quickSort(array, index, right);
            
        }
    }
    return array;
};


quickSort_btn.addEventListener("click", async function(){
    var sorted_array = await quickSort(unsortedArray, 0, (unsortedArray.length-1));
    console.log(sorted_array);
});

//----------------------------------------Merge Sort-------------------------------------------->
async function merge(array, low, mid, high){
    const n1 = mid - low + 1;
    const n2 = high - mid;
    let leftArr = new Array(n1);
    let rightArr = new Array(n2);
    let bars = document.getElementsByClassName("bar");

    for (let i = 0; i < n1; i++) {
        await waitforme(speed);
        bars[low + i].style.backgroundColor="orange"
        leftArr[i] = array[low + i];
    
    }
    for (let i = 0; i < n2; i++) {
        await waitforme(speed);
        bars[mid+1+i].style.backgroundColor="yellow";
        rightArr[i] = array[mid + 1 + i];
       
        
    }
    await waitforme(speed);
    let i = 0, j = 0, k = low;
    while(i < n1 && j < n2){
        await waitforme(speed);
        if(parseInt(leftArr[i]) <= parseInt(rightArr[j])){
            if ((n1 + n2) === array.length){
                bars[k].style.backgroundColor="green";
            }
            else{
                bars[k].style.backgroundColor="lightgreen";
            }
            array[k] = leftArr[i];
            bars[k].style.height = leftArr[i]* 12 + "px";
            i++;
            k++;
        }
        else{
            if ((n1 + n2) === array.length){
                bars[k].style.backgroundColor="green";
            }
            else{
                bars[k].style.backgroundColor="lightgreen";
            }
            array[k] = rightArr[j];
            bars[k].style.height = rightArr[j]* 12 + "px";
            j++;
            k++;
        }
    }
    while(i < n1){
        await waitforme(speed);
        if ((n1 + n2) === array.length){
            bars[k].style.backgroundColor="green";
        }
        else{
            bars[k].style.backgroundColor="lightgreen";
        }
        array[k] = leftArr[i];
        bars[k].style.height = leftArr[i]* 12 + "px";
        i++;
        k++;
    }
    while(j < n2){
        await waitforme(speed);
        if ((n1 + n2) === array.length){
            bars[k].style.backgroundColor="green";
        }
        else{
            bars[k].style.backgroundColor="lightgreen";
        }
        array[k] = rightArr[j];
        bars[k].style.height = rightArr[j]* 12 + "px";
        j++;
        k++;
    }
}

async function mergeSort(array, left, right){
    if(left >= right){
        return;
    }
    const mid = left + Math.floor((right - left) / 2);
    await mergeSort(array, left, mid);
    await mergeSort(array, mid + 1, right);
    await merge(array, left, mid, right);
}

mergeSort_btn.addEventListener("click", function(){
    let sorted_array = mergeSort(unsortedArray, 0, (unsortedArray.length) - 1);
    console.log(sorted_array);
});
