---
emoji: ๐ช
title: (์๊ณ ๋ฆฌ์ฆ) ์ธ๊ทธ๋จผํธ ํธ๋ฆฌ ์๊ณ ๋ฆฌ์ฆ + ์์  ์ฝ๋
date: '2019-07-13 04:00:00'
author: ์ค์ฝ๋ฉ
tags: algorithm
categories: ์๊ณ ๋ฆฌ์ฆ
---

## ์ธ๊ทธ๋จผํธ ํธ๋ฆฌ๋?

- ์ธ๊ทธ๋จผํธ ํธ๋ฆฌ๋ ํธ๋ฆฌ์ ๊ฐ ๋ธ๋์ ์ด๋ ์ด ๋ถ๋ถ๋ถ๋ถ์ ์ฐ์ฐ ๊ฒฐ๊ณผ๋ฅผ ๋ฏธ๋ฆฌ ์ ์ฅํด๋์ผ๋ฏ๋ก์จ **ํ์ ์๊ฐ์ OlogN)์ผ๋ก ๊ฐ์**์์ผ์ค๋ค.
- ์ฃผ๋ก ๋ถ๋ถํฉ, ๋ถ๋ถ์ต์, ์ต๋๊ฐ์ ์ฐพ๋๋ฐ ์ฌ์ฉ๋๋ค.

## ์ธ๊ทธ๋จผํธ ํธ๋ฆฌ ๊ตฌ์กฐ

- ์ธ๊ทธ๋จผํธ ํธ๋ฆฌ์ ๋ฆฌํ ๋ธ๋์ ๋๋จธ์ง ๋ธ๋๋ ๋ค์๊ณผ ๊ฐ์ ์๋ฏธ๋ฅผ ๊ฐ์ง๋ค.
- ๋ฆฌํ ๋ธ๋ : ํด๋น ์ด๋ ์ด ๊ฐ
- ๋ค๋ฅธ ๋ธ๋ : ์ค๋ฅธ์ชฝ ์์๊ณผ ์ผ์ชฝ ์์์ ์ฐ์ฐํ ๊ฒฐ๊ณผ ๊ฐ

๊ตฌ์กฐ๋ ์๋์ ๊ฐ์ ๊ตฌ์กฐ๋ฅผ ๊ฐ์ง๊ฒ ๋๋ค.

![์ฌ์ง](https://raw.githubusercontent.com/zoomKoding/zoomKoding.github.io/source/assets/_posts/segment-tree-1.png)

## ์ธ๊ทธ๋จผํธ ํธ๋ฆฌ ๋ง๋ค๊ธฐ(ํฉ ๊ตฌํ๊ธฐ์ฉ)

```cpp
// a: ๋ฐฐ์ด a
// tree: ์ธ๊ทธ๋จผํธ ํธ๋ฆฌ
// node: ์ธ๊ทธ๋จผํธ ํธ๋ฆฌ ๋ธ๋ ๋ฒํธ
// node๊ฐ ๋ด๋นํ๋ ํฉ์ ๋ฒ์๊ฐ start ~ end
long long init(vector<long long> &a, vector<long long> &tree, int node, int start, int end) {
    if (start == end) {
        return tree[node] = a[start];
    } else {
        return tree[node] = init(a, tree, node*2, start, (start+end)/2) + init(a, tree, node*2+1, (start+end)/2+1, end);
    }
}
```

๋ง์ฝ node๋ฅผ root node๋ก ์ฃผ๊ณ  start, end๋ฅผ ์ด๋ ์ด ์ ์ฒด ๋ฒ์๋ก ์ก๊ฒ ๋๋ฉด ๋ฆฌ์ปฌ์์ ํตํด tree๊ฐ ์์ฑ ๋๋ค.

## ์ธ๊ทธ๋จผํธ ํธ๋ฆฌ ํ์(ํฉ ๊ตฌํ๊ธฐ์ฉ)

๋ง์ฝ ์ด๋ ์ด ์์๊ฐ 10๊ฐ๋ผ๊ณ  ํ  ๋ 0-9๊น์ง ๋ชจ๋  ์์์ ํฉ์ ์ฐพ๊ณ  ์ถ๋ค๊ณ  ํ๋ฉด ์๋์ ๊ฐ์ด ๋ฃจํธ ๋ธ๋๋ง ํ์ธํ๋ฉด ๋  ๊ฒ์ด๋ค.

![์ฌ์ง](https://raw.githubusercontent.com/zoomKoding/zoomKoding.github.io/source/assets/_posts/segment-tree-2.png)

ํ์ง๋ง ๋ง์ฝ์ 2~4๊น์ง ๋ฒ์์ ํฉ์ ๊ตฌํ๊ณ  ์ถ๋ค๊ณ  ํ๋ฉด ์๋์ ๊ฐ์ด ๋๊ฐ์ ์์๋ฅผ ํ์ธํ๋ฉด ๋  ๊ฒ์ด๋ค.

![์ฌ์ง](https://raw.githubusercontent.com/zoomKoding/zoomKoding.github.io/source/assets/_posts/segment-tree-3.png)

๊ทธ๋์ ๋ง์ฝ ๋ธ๋๊ฐ ๋ด๋นํ๋ ๊ตฌ๊ฐ์ [start, end] ๋ผ๊ณ  ํ๊ณ  ํฉ์ ๊ตฌํด์ผํ๋ ๊ตฌ๊ฐ์ [left, right] ์ด๋ผ ํ๋ฉด ๊ฒฝ์ฐ๋ 4๊ฐ์ง๊ณ  ๋๋์ด ์ง๋ค.

1. [left, right]์ [start, end]๊ฐ ๊ฒน์น์ง ์๋ ๊ฒฝ์ฐ
2. [left, right]๊ฐ [start, end]๋ฅผ ์์  ์ปค๋ฒํ๋ ๊ฒฝ์ฐ
3. [start, end]๊ฐ [left, right]๋ฅผ ์์ ํ ์ปค๋ฒํ๋ ๊ฒฝ์ฐ
4. [left, right]์ [start, end]๊ฐ ๋ถ๋ถ์ ์ผ๋ก ๊ฒน์ณ์ ธ ์๋ ๊ฒฝ์ฐ

- 1๋ฒ : ๋ฌด๊ดํ๊ธฐ ๋๋ฌธ์ ํฉ์ ๊ตฌํ๋ค๊ณ  ํ๋ฉด 0์ returnํ๋ฉด ๋๋ค.
- 2๋ฒ : ๋์ด์ ํ์ํด๋ ์๋ฏธ๊ฐ ์๊ธฐ ๋๋ฌธ์ ํ์ฌ ๋ธ๋ ๊ฐ์ returnํ๋ค.
- 3๋ฒ,4๋ฒ : ๋ ์ชผ๊ฐ์ ๋ค์ ํ์์ ์งํํ๋ค.

์ฝ๋๋ ์๋์ ๊ฐ๋ค.

```cpp
// node๊ฐ ๋ด๋นํ๋ ๊ตฌ๊ฐ์ด start~end์ด๊ณ , ๊ตฌํด์ผํ๋ ํฉ์ ๋ฒ์๋ left~right
long long sum(vector<long long> &tree, int node, int start, int end, int left, int right) {
    if (left > end || right < start) {
        return 0;
    }
    if (left <= start && end <= right) {
        return tree[node];
    }
    return sum(tree, node*2, start, (start+end)/2, left, right) + sum(tree, node*2+1, (start+end)/2+1, end, left, right);
}
```

## 6549๋ฒ ํ์คํ ๊ทธ๋จ์์ ๊ฐ์ฅ ํฐ ์ง์ฌ๊ฐํ

์ด ๋ฌธ์ ์์๋ ๊ฐ segment์ ์ต์๊ฐ์ ์ ์ฅํด์ **์ต์๊ฐ์ ๋ ๋นจ๋ฆฌ ์ฐพ๊ธฐ ์ํด** ์ธ๊ทธ๋จผํธ ํธ๋ฆฌ๊ฐ ์ฌ์ฉ๋์๋ค.

์ฝ๋๋ ๋ค์๊ณผ ๊ฐ๋ค.

```cpp
#include <cstdio>
#include <vector>
#include <cmath>

using namespace std;
int n, input, answer = 0;

int find_min(vector<int> &a, vector<int> &tree, int node, int start, int end, int left, int right){
    if (left > end || right < start) return -1;
    if (left <= start && end <= right) return tree[node];

    int c1 = find_min(a, tree, node*2, start, (start+end)/2, left, right);
    int c2 = find_min(a, tree, node*2+1, (start+end)/2+1, end, left, right);
    if(c1 == -1) return c2;
    if(c2 == -1) return c1;
    return a[c1] < a[c2] ? c1 : c2;

}

long long div_and_conq(int left, int right, vector<int> &v, vector<int> &tree){
    int min = find_min(v, tree, 1, 0, v.size()-1, left, right);
    long long box = (long long)v[min]*(long long)(right-left+1);
    if(left <= min - 1){
        long long temp = div_and_conq(left, min -1, v, tree);
        if(box < temp) box = temp;

    }
    if(min + 1 <= right) {
        long long temp = div_and_conq(min + 1, right, v, tree);
        if(box < temp) box = temp;
    }
    return box;
}

// a: ๋ฐฐ์ด a
// tree: ์ธ๊ทธ๋จผํธ ํธ๋ฆฌ
// node: ์ธ๊ทธ๋จผํธ ํธ๋ฆฌ ๋ธ๋ ๋ฒํธ
// node๊ฐ ๋ด๋นํ๋ ํฉ์ ๋ฒ์๊ฐ start ~ end

void init(vector<int> &a, vector<int> &tree, int node, int start, int end){
    if(start == end) tree[node] = start;
    else{
        init(a, tree, node*2, start, (start+end)/2);
        init(a, tree, node*2 + 1, (start+end)/2 + 1, end);
        tree[node] = a[tree[node*2]] < a[tree[node*2+1]] ? tree[node*2] : tree[node*2+1];
    }
}

int main(){
    while(1){
        scanf("%d", &n);
        if(n == 0) break;
        answer = 0;
        int h = (int)(ceil(log2(n))+1e-9);
        int tree_size = (1 << (h+1));

        vector<int> v(n);
        vector<int> tree(tree_size);

        for(int i = 0; i < n; i++)scanf("%d", &v[i]);
        init(v, tree, 1, 0, n-1);
        printf("%lld\n", div_and_conq(0, n-1, v, tree));
    }
}
```

## ์ฃผ์ํ ์ 

- ๋ฒกํฐ๋ฅผ ๋ณต์ฌํ  ๊ฑฐ ์๋๋ฉด parameter๋ก ์ธ ๋ &๋ฅผ ๋ถ์ฌ์ค๋ผ. ๋ณต์ฌํ๋๋ฐ ์๊ฐ์ด ๋ ์๋ชจ๋๋ค.
