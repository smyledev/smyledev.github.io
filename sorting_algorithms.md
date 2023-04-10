# Sorting algorithms

| № |          Algorithm           | Time complexity (Worst) | Time complexity (Average) |   Time complexity (Best)  | Space compexity |
|:-:|:----------------------------:|:-----------------------:|:-------------------------:|:-------------------------:|:---------------:|
| 1 |    [Bubble sort](#Bubble)    |          O(n^2)         |          O(n^2)           |            O(n)           |       O(1)      |
| 2 | [Insertion sort](#Insertion) |          O(n^2)         |          O(n^2)           |            O(n)           |       O(n)      |
| 3 |     [Quick sort](#Quick)     |          O(n^2)         |         O(n log n)        |            O(n)           |       O(n)      |
| 4 |     [Merge sort](#Merge)     |       O(n log n)        |         O(n log n)        |         O(n log n)        |       O(n)      |


<br/>


### <a name="Bubble"></a> Bubble sort

```c++
void BubbleSort(vector<int>& values) {
  for (size_t idx_i = 0; idx_i + 1 < values.size(); ++idx_i) {
    for (size_t idx_j = 0; idx_j + 1 < values.size() - idx_i; ++idx_j) {
      if (values[idx_j + 1] < values[idx_j]) {
        swap(values[idx_j], values[idx_j + 1]);
      }
    }
  }
}
```

<br/>

### <a name="Insertion"></a> Insertion sort

```c++
void InsertionSort(vector<int>& values) {
  for (size_t i = 1; i < values.size(); ++i) {
    int x = values[i];
    size_t j = i;
    while (j > 0 && values[j - 1] > x) {
      values[j] = values[j - 1];
      --j;
    }
    values[j] = x;
  }
}
```

<br/>

### <a name="Quick"></a> Quick sort

```c++
int Partition(vector<int>& values, int l, int r) {
  int x = values[r];
  int less = l;
  for (int i = l; i < r; ++i) {
    if (values[i] <= x) {
      swap(values[i], values[less]);
      ++less;
    }
  }
  swap(values[less], values[r]);
  return less;
}
void QuickSortImpl(vector<int>& values, int l, int r) {
  if (l < r) {
    int q = Partition(values, l, r);
    QuickSortImpl(values, l, q - 1);
    QuickSortImpl(values, q + 1, r);
  }
}
void QuickSort(vector<int>& values) {
  if (!values.empty()) {
    QuickSortImpl(values, 0, values.size() - 1);
  }
}
```

<br/>

### <a name="Merge"></a> Merge sort

```c++
void MergeSortImpl(vector<int>& values, vector<int>& buffer, int l, int r) {
  if (l < r) {
    int m = (l + r) / 2;
    MergeSortImpl(values, buffer, l, m);
    MergeSortImpl(values, buffer, m + 1, r);
    int k = l;
    for (int i = l, j = m + 1; i <= m || j <= r; ) {
      if (j > r || (i <= m && values[i] < values[j])) {
        buffer[k] = values[i];
        ++i;
      } else {
        buffer[k] = values[j];
        ++j;
      }
      ++k;
    }
    for (int i = l; i <= r; ++i) {
      values[i] = buffer[i];
    }
  }
}
void MergeSort(vector<int>& values) {
  if (!values.empty()) {
    vector<int> buffer(values.size());
    MergeSortImpl(values, buffer, 0, values.size() - 1);
  }
}
```

[Назад](./)
