---
emoji: ๐ช
title: (์๊ณ ๋ฆฌ์ฆ) Knapsack ์๊ณ ๋ฆฌ์ฆ 2 Branch and Bound, Heap + ์ฝ๋
date: '2019-04-20 02:00:00'
author: ์ค์ฝ๋ฉ
tags: algorithm knapsack
categories: ์๊ณ ๋ฆฌ์ฆ
---

## Branch and Bound?

Branch(๊ฐ์ง)์ Bound(๋ฒ์)๋ฅผ ์ด์ฉํ ๋ฐฉ๋ฒ์ผ๋ก ์ต์ ์ ํด๋ฅผ ์ฐพ๊ธฐ ์ํด ์ด๋ ์ ๋์ ๋ฒ์๋ฅผ ์ ํด๋๊ณ  ๋ฒ์๋ฅผ ๋ฒ์ด๋ ๊ฐ๋ค์ ๊ฐ์ง์น๊ธฐ ํ๋ ๋ฐฉ๋ฒ์ ์๋ฏธํ๋ค.

BFS๋ฅผ ์ด์ฉํด ๋์ค๋ฅผ ๋๋ ค๊ฐ๋ฉฐ ์ต์ ์ ๊ฐ์ ์ฐพ๋๋ค๊ณ  ํ์ ๋ ๋ชจ๋  ๋ฆฌํ๊น์ง ๊ฐ์ง ์๊ณ  ์ด๋์ ๋์ ๋ฐ์ด๋๋ฆฌ๋ฅผ ์ ํ๊ณ  ๋ฐ์ด๋๋ฆฌ ๋ฐ์ ์๋ ์น๊ตฌ๋ค์ ์ ํ๋ ๋ฐฉ๋ฒ์ ์๋ฏธํ๋ค.

### Knapsack์ ์ด๋ป๊ฒ ์ ์ฉํด?

- ๊ฐ ์์๋ฅผ ๋ฃ๋ ๊ฒฝ์ฐ์ ์ ๋ฃ๋ ๊ฒฝ์ฐ๋ก ๋ ๊ฒฝ์ฐ๋ก ๊ฐ์ง(Branch)๋ฅผ ์ณ๊ฐ๋๋ฐ
- ํ์ฌ ์์์ ์ต๋์น๋ก ๋ฃ์์ ๋์ ๊ฒฝ์ฐ(bound)๊ฐ ํ์ฌ ์ฐพ์ ์ต๋ ๊ฐ์น(max_benefit)์ ๋์ง ๋ชปํ๋ฉด ๋ ์ด์ ๋ณผ ๊ฐ์น๊ฐ ์๋ ์น๊ตฌ์ด๋ฏ๋ก ๋์ด์ ๋ป์ง ์๋๋ค.(queue์์ ๋ฃ์ง ์๋๋ค.)
- ์ฌ๊ธฐ์ queue๋ฅผ priority queue๋ก ํ๋ฉด ๋งฅ์ค ๊ทผ์ฒ ๊ฐ์ ๋ ๋นจ๋ฆฌ ์ฐพ์ ์ ์๊ธฐ ๋๋ฌธ์ ๊ฐ์น๊ฐ ์๋ ์น๊ตฌ๋ค์ ๋ ๋ง์ด ๊ฑธ๋ฌ๋ผ ์ ์๋ค!

๊ฐ์น๊ฐ ์๋ค๊ณ  ์ฌ๊ธฐ๋ ๊ฒฝ์ฐ๋ ๋๊ฐ์ง์ด๋ค.

1. bound <= max_benefit (์ด ๊ฐ์ง๋ก ๋ป์ณค์ ๋ ์ต๋ ๊ธฐ๋๊ฐ์น๊ฐ ํ์ฌ ๋งฅ์ค๋ณด๋ค ์์๋ pass)
2. weight > W (ํ์ฌ ์์ดํ์ ๋ฃ์ผ๋ฉด ๋์ ์ฉ๋์ด ์ต๊ฐ๋๋ค๋ฉด pass)

๊ทธ๋ ๋ค๋ฉด ์ฝ๋๋ฅผ ํ๋ฒ ๋ณด์

**Branch and Bound ์ฝ๋**

```cpp
typedef struct NodeStruct
{
  int weight;
  int benefit;
  int index;
  float bound;
} Node;

#define MAX_SIZE 10000

Node heap[MAX_SIZE];
int size;

void heap_init() {
    size = 0;
}

void heap_swap(Node *a, Node *b) {
    Node tmp = *a;
    *a = *b;
    *b = tmp;
}

int push(Node value) {
    if (size + 1 > MAX_SIZE) {
        return 0;
    }

    heap[size] = value;

    int current = size;
    int parent = (size - 1) / 2;

    while (current > 0 && heap[current].bound > heap[parent].bound) {
        heap_swap(&heap[current], &heap[parent]);
        current = parent;
        parent = (parent - 1) / 2;
    }

    size++;

    return 1;
}

Node pop() {
    if (size <= 0) {
        Node temp;
        temp.benefit = -1;
        return temp;
    }

    Node ret = heap[0];
    size--;

    heap[0] = heap[size];
    int current = 0;
    int leftChild = current * 2 + 1;
    int rightChild = current * 2 + 2;
    int maxNode = current;

    while (leftChild < size) {
        if (heap[maxNode].bound < heap[leftChild].bound) {
            maxNode = leftChild;
        }
        if (rightChild < size && heap[maxNode].bound < heap[rightChild].bound) {
            maxNode = rightChild;
        }

        if (maxNode == current) {
            break;
        }
        else {
            heap_swap(&heap[current], &heap[maxNode]);
            current = maxNode;
            leftChild = current * 2 + 1;
            rightChild = current * 2 + 2;
        }
    }

    return ret;
}


float bandb(Item* array, int n, int w){
    heap_init();
    float max_benefit = 0.0;
    int weight = 0;
    //queue ์ฌ์ด์ฆ ํ ๋นํด์ฃผ๊ธฐ
    Node root;
    //root node ๋ฃ์ด์ค
    root.weight = 0;
    root.benefit = 0;
    root.index = 0;
    for(int i = 0; i < n; i++){
        if(weight+array[i].weight > w){
            root.bound += (w-weight)*array[i].value;
            break;
        }
        else {
            root.bound += array[i].benefit;
            weight += array[i].weight;
        }
    }
    push(root);

    //q์์ ๊บผ๋ธ ์์๊ฐ -1์ด ์๋๋ผ๋ฉด ๋ฐ๋ณต
    while(1){
        Node temp = pop();
        Node child[2];
        if(temp.benefit == -1) break;


        int index = temp.index + 1;


        if(temp.bound < max_benefit) continue;

        if(temp.index == n-1)continue;

        child[0].weight = temp.weight + array[index-1].weight;
        child[0].benefit = temp.benefit + array[index-1].benefit;
        child[1].weight = temp.weight;
        child[1].benefit = temp.benefit;

        //๋ฃ๊ธฐ๋ก ํ๊ณ  ํ๋๋ ์๋ฃ๊ธฐ๋กํด์ ๋๋ฒ ์์ํด
        //๊บผ๋ธ ์์์ weight๊ณผ w๋ฅผ ๋น๊ตํด์ ๋์ผ๋ฉด continue
        for(int i = 0; i < 2; i++){

            child[i].index = index;
            child[i].bound = child[i].benefit;

            if(w < child[i].weight) continue;

            //๊บผ๋ธ ์์์ bound๋ฅผ ๊ตฌํ๊ณ 
            weight = child[i].weight;
            for(int j = index; j < n; j++){
                if(weight+array[j].weight > w){
                    child[i].bound += (w-weight)*array[j].value;
                    break;
                }
                else {
                    child[i].bound += array[j].benefit;
                    weight += array[j].weight;
                }
            }
            //max_benefit๋ณด๋ค ์์ผ๋ฉด continue
            if(child[i].bound < max_benefit) continue;

            //benefit์ ๊ตฌํ๊ณ  max_benefit ๋ณด๋ค ํฌ๋ฉด max_benefit ์๋ฐ์ดํธ
            if(child[i].benefit > max_benefit)max_benefit = child[i].benefit;
            //push ํด์ค
            push(child[i]);

        }
    }

    //๋ค๋์์ผ๋ฉด ๊ฒฐ๊ณผ ๋ฆฌํดํด
    return max_benefit;

}
```

- Heap์ ๊ตฌํํด์ ์ฌ์ฉํ๋ค.
- ์ต์ด root ๊ฐ์ ๋ง๋ค์ด์ ๋ฃ์ด์ค๋ค.
- heap์์ ํ๋์ฉ ๊บผ๋ด์ child๋ฅผ ๋ง๋ค์ด์ heap์ ๋ฃ์ด์ฃผ๊ธฐ๋ฅผ ๋ฐ๋ณตํ๋ค.
- ๋ฐ์ด๋๋ฅผ ํต๊ณผํ ์น๊ตฌ๋ค๋ง heap์ ๋ค์ด๊ฐ๋ค.

## ๋๋์ 

- ์ง์ง ์ฝ๋ฉ์ ๋ฐฉ๋ฒ์ ๋ง๋ ์๋๊ฒ ๋ค์ํ๋ค.
- ์ฒ์์ ๊ทธ๋ฅ queue๋ฅผ ์ผ๋ค๊ฐ ์ ์ง์ด ๋ง๋ฃ๊ณ  heap์ผ๋ก ๋ฐ๊ฟจ๋๋ฐ ์๊ฐ ์ฐจ์ด๊ฐ ๋ง๋ ์๋๊ฒ ๋ง์ด ๋๋ค.
- ์ง์ง... ๋ค์์๋ ํ๋ ๊ณต๋ถํด๋ณธ๋ค.. ์ธ์์๋ ๋ค์ํ ๋ฐฉ๋ฒ์ด ์กด์ฌํ๋ค ์ ๋ง๋ฃจ ์์ง ๋ฉ์๋ค.
