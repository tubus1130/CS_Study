## 📓 키워드

- LRU

---

## ✏️ LRU

1. 페이지가 캐시에 있는 지를 확인
2. 만약 없다면 가장 참조가 오래된 페이지와 참조를 하려는 페이지를 스와핑

### 💭 로직

- 가장 오래된 것 : 연결리스트의 가장 마지막 요소
- 최근 참조된 것 : 연결리스트의 첫번째 요소

```cpp
#include <bits/stdc++.h>
using namespace std;
class LRUCache {
    list<int> li;
    unordered_map<int, list<int>::iterator> hash;
    int csize;
public:
    LRUCache(int);
    void refer(int);
    void display();
};

LRUCache::LRUCache(int n){
    csize = n;
}

void LRUCache::refer(int x){
    if (hash.find(x) == hash.end()) { // 캐시에 없으면
        if (li.size() == csize) {
            int last = li.back();
            li.pop_back();
            hash.erase(last);
        }
    }else { // 히트
        cout << "해당 페이지를 제거!! : " << distance(li.begin(),
        hash[x]) << "번째 " << *hash[x] << '\n';
        li.erase(hash[x]);
    }
    li.push_front(x);
    hash[x] = li.begin();
}

void LRUCache::display(){
    for (auto it = li.begin(); it != li.end(); it++){
        cout << (*it) << " ";
    }
    cout << "\n";
}

vector<int> test_page = {1, 3, 0, 3, 5, 6, 3};

int main(){
    LRUCache ca(3);
    for(int i : test_page){
        ca.refer(i); ca.display();
    }
    return 0;
}

/*
1
3 1
0 3 1
해당 페이지를 제거!! : 1번째 3
3 0 1
5 3 0
6 5 3
해당 페이지를 제거!! : 2번째 3
3 6 5
*/
```