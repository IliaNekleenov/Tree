# Tree  
```cpp  
template <typename T, class Compare = std::less<>>  
struct Heap {  
  std::vector<T> data;  
  size_t N;  
  Heap() {  
    data.resize(1);  
    data[0] = 0;  
    N = 0;  
  }  
  void add(const T& a, Compare cmp = Compare{}) {  
    data.push_back(a);  
    N++;  
    size_t p = N;  
    while (p > 1 && cmp(data[p / 2], data[p])) {  
      T buf = data[p];  
      data[p] = data[p / 2];  
      data[p / 2] = buf;  
      p /= 2;  
    }  
  }  
  T remove_max(Compare cmp = Compare{}) {  
    T removed = data[1];  
    data[1] = data[N];  
    N--;  
    size_t p = 1;  
    bool moved = true;  
    while (moved && 2 * p <= N) {  
      moved = false;  
      if (cmp(data[2 * p], data[2 * p + 1])) {  
        if (cmp(data[p], data[2 * p + 1])) {  
          moved = true;  
          T buf = data[p];  
          data[p] = data[2 * p + 1];  
          data[2 * p + 1] = buf;  
          p = 2 * p + 1;  
        }  
      } else {  
        if (cmp(data[p], data[2 * p])) {  
          moved = true;  
          T buf = data[p];  
          data[p] = data[2 * p];  
          data[2 * p] = buf;  
          p = 2 * p;  
        }  
      }  
    }  
    return removed;  
  }  
};  
  
template <class It, class Compare = std::less<>>  
void heap_sort(It first, It last, Compare cmp = Compare{}) {  
  Heap<typename std::iterator_traits<It>::value_type, Compare> heap;  
  auto root = first;  
  while (root != last) {  
    heap.add(*root, cmp);  
    root++;  
  }  
  while (root != first) {  
    root--;  
    *root = heap.remove_max(cmp);  
  }  
}
