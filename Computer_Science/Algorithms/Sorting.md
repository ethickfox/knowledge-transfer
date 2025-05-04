# **Алгоритмы**

**Сложность алгоритмов -** Big O. С помощью этого значения мы определяем эффективность того или иного решения проблемы.

И вместо времени выполнения мы начинаем думать о том, какое соотношение времени выполнения к входным данным. При этом, размер входных данных мы обозначаем, как n.

Таким образом, если время выполнения напрямую зависит от размера входных данных, то эта ситуация выражается математически, как O(n). Если мы, по какой-либо причине, решили сравнить каждое значение с каждым, то сложность будет n*n. Эта ситуация выражается, как O(n^2) (n в квадрате)

Here are some common types of time complexities in Big O Notation.

**O(1) - Constant time complexity**

Constant time algorithms will always take same amount of time to be executed. The execution time of these algorithm is independent of the size of the input. A good example of O(1) time is accessing a value with an array index.

**O(n) - Linear time complexity**

An algorithm has a linear time complexity if the time to execute the algorithm is directly proportional to the input size n. Therefore the time it will take to run the algorithm will increase proportionately as the size of input n increases.

A good example is finding a CD in a stack of CDs or reading a book, where n is the number of pages.

**O(log n) - Logarithmic time complexity**

An algorithm has logarithmic time complexity if the time it takes to run the algorithm is proportional to the logarithm of the input size n. An example is binary search, which is often used to search data sets

**O(n^2) - Quadratic time complexity**

An algorithm has quadratic time complexity if the time to execution it is proportional to the square of the input size. A good example of this is checking to see whether there are any duplicates in a deck of cards.

You will encounter quadratic time complexity in algorithms involving nested iterations, such as nested for loops. In fact, the deeper nested loops will result in O(n^3), O(n^4), etc.

[![](https://lh3.googleusercontent.com/6ZhSf-kwTWoSz5qMXYJz4G8LY-SB_AdAFxE1-51XsXNzXX2AF4oURBf8kC3gr14m-TxaZ8zNiK0VryvcHLpp-0440NIbXarUiFyEQ1SZW3YCObzgEyqr_bSja8IVD8_9j7v2AlAInRdFW4ASNB_-eESBW7RdwvEpDbUn5j13wh1wRZt4eZ3aPPFu7ND1)](https://lh3.googleusercontent.com/6ZhSf-kwTWoSz5qMXYJz4G8LY-SB_AdAFxE1-51XsXNzXX2AF4oURBf8kC3gr14m-TxaZ8zNiK0VryvcHLpp-0440NIbXarUiFyEQ1SZW3YCObzgEyqr_bSja8IVD8_9j7v2AlAInRdFW4ASNB_-eESBW7RdwvEpDbUn5j13wh1wRZt4eZ3aPPFu7ND1)

**Жадный алгоритм**

Алгоритм, который на каждом шагу делает локально наилучший выбор в надежде, что итоговое решение будет оптимальным.(алгоритм Дейкстры нахождения кратчайшего пути в графе)

**Алгоритм разделяй и властвуй**

Типичный алгоритм такого типа решает проблему с помощью трех шагов

- Разделить проблему на подброблемы одинакового типа
- Рекурсивно решить подпроблемы
- Объединить результат подброблем

**Linear Search**

Time:             Worst – O(n)          Average – O(n)                  Best – O(1)

Space:          Worst – O(1)

Linear search or sequential search is a method for finding an element within a list. It sequentially checks each element of the list until a match is found or the whole list has been searched. Linear search is rarely used practically because other search algorithms such as the binary search algorithm and hash tables allow significantly faster searching comparison to Linear search.

**Binary Search - Divide & Conquer**

Time:             Worst – O(log n)                Average – O(log n)             Best – O(1)

Space:          Worst – O(1)

Binary search, also known as half-interval search or logarithmic search, is a search algorithm that finds the position of a target value within a sorted array. Binary search compares the target value to the middle element of the array. If they are not equal, the half in which the target cannot lie is eliminated and the search continues on the remaining half, repeating this until the target value is found. If the search ends with the remaining half being empty, the target is not in the array.

**Depth-first search (DFS)**

Depth-first search is an algorithm for traversing or searching tree or graph data structures. The algorithm starts at the root node (selecting some arbitrary node as the root node in the case of a graph) and explores as far as possible along each branch before backtracking.

**Breadth-first search (BFS)**

Breadth-first search is an algorithm for traversing or searching tree or graph data structures. It starts at the tree root (or some arbitrary node of a graph), and explores all of the neighbor nodes at the present depth prior to moving on to the nodes at the next depth level.

**Sorting Algorithms**

**Bubble Sort (пузырьковая сортировка) - STABLE**

Time: Worst – O(n2)         Average – O(n2)                 Best – O(n)

Space:   Worst – O(1)

Bubble sort is a simple sorting algorithm that repeatedly steps through the list, compares adjacent pairs and swaps them if they are in the wrong order. The pass through the list is repeated until the list is sorted. One of the variations is a **shaker sort (коктейльная сортировка)**, where we path the array from both sides, solving a problem of small numbers in the end.

=======================================

public static void bubbleSort(int[] array) {

boolean sorted = false;

while(!sorted) {

sorted = true;

for (int i = 0; i < array.length - 1; i++) {

if (array[i] > array[i+1]) {

swap(array[i], array[i+1]);

sorted = false;

}

}

}

}

=======================================

Application: due to its simplicity, bubble sort is often used to introduce the concept of an algorithm.

**Selection Sort (сортировка выбором) - unstable**

Time: Worst – O(n2)         Average – O(n2)                 Best – O(n2)

Space:   Worst – O(1)

selection sort is a sorting algorithm, specifically an in-place comparison sort. The algorithm divides the input list into two parts: the sublist of items already sorted, which is built up from left to right at the front (left) of the list, and the sublist of items remaining to be sorted that occupy the rest of the list. Initially, the sorted sublist is empty and the unsorted sublist is the entire input list. The algorithm proceeds by finding the smallest (or largest, depending on sorting order) element in the unsorted sublist, swapping it with the leftmost unsorted element (putting it in sorted order), and moving the sublist boundaries one element to the right.

=======================================

public static void selectionSort(int[] array) {

for (int i = 0; i < array.length; i++) {

int min = array[i];

int minId = i;

for (int j = i+1; j < array.length; j++) {

if (array[j] < min) {

min = array[j];

minId = j;

}

}

swap(array[i], array[minId]);

}

}

=======================================

Application: Small lists and limited memory. Used in some real-time applications.

**Insertion Sort (сортировка вставками) - STABLE**

Time: Worst – O(n2)         Average – O(n2)                 Best – O(n)

Space:   Worst – O(1)

Insertion sort is a simple sorting algorithm that builds the final sorted array (or list) one item at a time. Insertion sort iterates, consuming one input element each repetition, and growing a sorted output list. At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there. It repeats until no input elements remain. Like sorting cards in a bridge.

=======================================

public static void insertionSort(int[] array) {

for (int i = 1; i < array.length; i++) {

int current = array[i];

int j = i - 1;

while(j >= 0 && current < array[j]) {

array[j+1] = array[j];

j--;

}

array[j+1] = current;

}

}

=======================================

Application: Small lists and limited memory, best  with already sorted array or close-to-sorted array. Practically more efficient comparing to bubble sort and selection sort.

**Merge Sort (сортировка слиянием) - STABLE, Divide & Conquer**

Time: Worst – O(n log n)     Average – O(n log n)          Best – O(n log n)

Space:   Worst – O(n)

A merge sort works as follows:

1. Divide the unsorted list into n sublists, each containing 1 element (a list of 1 element is considered sorted).

2. Repeatedly merge sublists to produce new sorted sublists until there is only 1 sublist remaining. This will be the sorted list.

Application: Large lists, best to sort linked list. More efficient than quick sort at handling slow-to-access sequential media such as disk storage or network-attached storage.

**Quick Sort (быстрая сортировка) - unstable, Divide & Conquer**

Time: Worst – O(n2)         Average – O(n log n)          Best – O(n log n)

Space:   Worst – O(log n)

Quicksort first divides a large array into two smaller sub-arrays: the low elements and the high elements. The steps are:

1. Pick an element, called a pivot, from the array.

2. Reorder the array so that all elements with values less than the pivot come before the pivot, while all elements with values greater than the pivot come after it. After this partitioning, the pivot is in its final position. This is called the **partition** operation.

3. Recursively apply the above steps to the sub-array of elements with smaller values and separately to the sub-array of elements with greater values.

Application: Large lists, best when pivot divide in 2 equal halves, not efficient for linked lists (poor pivot choice because of no random access). Generally outperform merge sort for sorting RAM-based arrays.

**Dual-pivot Quick Sort - unstable**

Dual-pivot quicksort делит массив на три отрезка, вместо двух. В результате количество операций перемещения элементов массива существенно сокращается.

**Heap Sort (пирамидальная сортировка) - unstable**

Time: Worst – O(n log n)             Average – O(n log n)          Best – O(n log n)

Space:   Worst – O(1)

The heapsort algorithm involves preparing the list by first turning it into a max heap. The algorithm then repeatedly swaps the first value of the list with the last value, decreasing the range of values considered in the heap operation by one, and sifting the new first value into its position in the heap. This repeats until the range of considered values is one value in length.The steps are:

1. Call the buildMaxHeap() function on the list. Also referred to as heapify(), this builds a heap from a list in O(n) operations.

2. Swap the first element of the list with the final element. Decrease the considered range of the list by one.

3. Call the siftDown() function on the list to sift the new first element to its appropriate index in the heap.

4. Go to step (2) unless the considered range of the list is one element.

Application: Large lists, the only advantage to quick sort by a smaller worst-case runtime. Comparing to merge sort, heapsort typically runs faster in practice on machines with small or slow data cash. Often used in embedded systems with real-time constraints or systems concerned with security.

**LSD Radix Sort (цифровая сортировка) (LSD – least significant digit) - STABLE**

Time: Worst – O(w * n)             Average – O(?)          Best – O(n)

Space:   Worst – O(w + n)

W – number of bits required to store each key

Radix sort is a non-comparative integer sorting algorithm that sorts data with integer keys by grouping keys by the individual digits which share the same significant position and value. A positional notation is required, but because integers can represent strings of characters (e.g., names or dates) and specially formatted floating point numbers, radix sort is not limited to integers.

**MSD Radix Sort (MSD – most significant digit) - STABLE**

Same as LSD, but starting with most significant digit.

**Tree Sort (сортировка с помощью бинарного дерева) - STABLE**

Time: Worst – O(n log n)         Average – O(n log n)                 Best – O(n)

Space:   Worst – O(n)

- см структуру данных Binary Tree

**ShellSort (сортировка Шелла) - unstable**

Time: Worst – O(n log 2 n)         Average – O(depends on step)     Best – O(n2)

Space:   Worst – O(1)

Modification of insertion sort. Идея метода Шелла состоит в сравнении элементов, стоящих не только рядом, но и на определённом расстоянии друг от друга. Иными словами — это сортировка вставками с предварительными «грубыми» проходами. Каждый проход в алгоритме характеризуется смещением hi, таким, что попарно сортируются элементы отстающие друг от друга на hi позиций.

Невзирая на то, что сортировка Шелла во многих случаях медленнее, чем быстрая сортировка, она имеет ряд преимуществ:

- отсутствие потребности в памяти под стек;
- отсутствие деградации при неудачных наборах данных — быстрая сортировка легко деградирует до O(n²)

Application: ?

**Counting Sort - STABLE**

Time: Worst – O(n + k), where k is the range of the non-negative key values

Space:   Worst – O(n + k)

Алгоритм сортировки объектов по ключам, представленным малыми численными значениями в определенном диапазоне. Метод основан на подсчете количества объектов с определенным значением. После подсчета количества производятся несложные вычисления и определяется порядок в результирующей коллекции.

Application: В ситуациях, когда различия в разнообразии ключевых значений не значительно превосходит количество элементов. Также иногда является сабрутиной для другой сортировки.

**Timsort – STABLE, Divide & Conquer**

Time: Worst – O(n log n)         Average – O(n log n)     Best – O(n)

Space:   Worst – O(n)

Гибридный алгоритм сортировки, включающий в себя сортировку вставкой и сортировку слиянием. Идея заключается в том, что в реальном мире сортируемые массивы данных часто содержат в себе упорядоченные подмассивы. На таких массивах тимсорт лучше многих других алгоритмов сортировки.

Алгоритм: по специальному алгоритму массив разделяется на подмассивы. Каждый подмассив сортируется сортировкой вставами. Отсортированные подмассивы сливаются в единый массив модифицированной сортировкой слиянием.

Application: В ситуациях, когда массив частично упорядочен в своих подмассивах.

## **Сортировки**

### **Устойчивые сортировки**

стабильная сортировка — сортировка, которая не меняет относительный порядок сортируемых элементов, имеющих одинаковые ключи.

**Сортировка пузырьком**

Bubble sort — для каждой пары индексов производится обмен, если элементы расположены не по порядку. Сложность алгоритма:O(n^2).

**Сортировка перемешиванием**

**Cocktail sort** - то же, что и пузырьковая, но ходит в обе стороны и сужает сортируемую зону с начала и конца. Сложность алгоритма: O(n^{2}).

**Сортировка вставками**

Insertion sort — проходим по всему массиву и определяем, где текущий элемент должен находиться в упорядоченном списке, и вставляем его туда. Тем самым увеличивая отсортированную часть Сложность алгоритма: O(n^{2}).

**Сортировка слиянием**

Merge sort — выстраиваем первую и вторую половину списка отдельно, а затем объединяем упорядоченные списки рекурсивно. Сложность алгоритма:O(n\log n). Требуется O(n) дополнительной памяти

.

**Сортировка с помощью двоичного дерева**

Tree sort -  Делаем дерево из элементов, где меньшие - слева, а большие справа.Заполняем массив слева направо. Сложность алгоритма: O(n\log n) в лучшем случае, а O(n^{2}) в худшем. Требуется O(n) дополнительной памяти.

### **Неустойчивые сортировки**

Меняет порядок элементов с одинаковыми ключами

**Сортировка расчёской**

Comb sort — То же что и пузырьком, но сравниваем не соседние, а к-во элементов/1.3 и    сложность алгоритма:O(n^{2};.

**Сортировка Шелла**

Shell sort — улучшение сортировки вставками. Сравниваем элементы через определенный промежуток. На каждой итерации промежуток делим на 2. сложность O(n\log ^{2}{n}).

**Пирамидальная сортировка**

Heapsort — сложность алгоритма:Превращаем список в кучу(n -родитель, n+1 и n+2 - дети ) Делаем макс хип и меняем больший и меньший элементы и забираем больший, опять приводим к макс хипO(n\logn); превращаем список в кучу, берём наибольший элемент и добавляем его в конец списка.

  

  

## **Эффективная сортировка**

### **Сортировка слиянием**

_**Определение:**_

- Сравнение данных с помощью алгоритма сортировки:
    - Весь набор данных делится минимум на две группы.
    - Пары значений сравниваются между собой, наименьшее перемещается влево.
    - После сортировки внутри всех пар, сравниваются левые значения двух левых пар. Таким образом, создаётся группа из четырёх значений: два наименьшие — слева, наибольшие — справа.
    - Процесс повторяется до тех пор, пока не останется только один набор.

_**Что вам нужно знать:**_

- Это один из фундаментальных алгоритмов сортировки.
- Данные делятся на как можно более маленькие наборы, которые потом сравниваются.

_**Эффективность («О» большое):**_

- Наилучший вариант сортировки: сортировка слиянием — O(n).
- Средний вариант сортировки: сортировка слиянием — O(n log n).
- Худший вариант сортировки: сортировка слиянием — O(n log n).

### **Быстрая сортировка**

_**Определение:**_

- Алгоритм сортировки на основе сравнения.
    - Весь набор данных делится пополам путём выбора среднего элемента и перемещения всех, кто меньше него, влево.
    - Затем такая же процедура итерационно выполняется с левой частью до тех пор, пока не останутся только два элемента. В результате левая часть окажется отсортированной.
    - Затем всё то же самое делается с правой частью.
- Этот алгоритм предпочитают использовать в архитектурах вычислительных систем.

_**Что вам нужно знать:**_

- Хотя «О» большое здесь имеет те же значения (а в ряде случаев — хуже), что и у многих других алгоритмов сортировки, но на практике этот алгоритм зачастую работает быстрее, например, той же сортировки слиянием.
- Данные будут последовательно делиться пополам, пока не будут целиком отсортированы.

_**Эффективность («О» большое):**_

- Наилучший вариант сортировки: быстрая сортировка — O(n).
- Средний вариант сортировки: быстрая сортировка — O(n log n).
- Худший вариант сортировки: быстрая сортировка — O(n^2).

### **Пузырьковая сортировка**

_**Определение:**_

- Алгоритм сортировки на основе сравнения.
    - Итерирует слева направо, сравнивая значения внутри каждой пары и перемещая наименьшее влево.
    - Процесс повторяется до тех пор, пока ни одно значение уже не может быть перемещено.

_**Что вам нужно знать:**_

- Алгоритм очень прост в реализации, но наименее эффективен из всех трёх, описанных здесь.
- Сравнив два значения и переместив наименьшее влево, алгоритм переходит на одну позицию вправо.

_**Эффективность («О» большое):**_

- Наилучший вариант сортировки: пузырьковая сортировка — O(n).
- Средний вариант сортировки: пузырьковая сортировка — O(n^2).
- Худший вариант сортировки: пузырьковая сортировка — O(n^2).

_**Сравнение алгоритмов сортировки слиянием и быстрой сортировки**_

- Быстрая сортировка на практике зачастую эффективнее.
- Сортировка слиянием сразу делит набор данных на наименьшие возможные группы, а затем восстанавливает набор, инкрементально сортируя и укрупняя группы.
- Быстрая сортировка последовательно делит набор по среднему значению, пока он не будет отсортирован рекурсивно.