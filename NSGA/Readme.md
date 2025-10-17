## Mục lục

# Giới thiệu về Thuật toán Non-Dominated Sorting Genetic Algorithm 2 (NSGA-II)

Thuật toán tiến hóa là một thuật toán giải gần đúng bài toán tối đa hóa đa mục tiêu.

## Cá thể trong NSGA-II

Mỗi cá thể trong NSGA-II có 2 thuộc tính:

- chromosome: Kiểu gen, là một vector
- fitness: Giá trị thích nghi, là một vector

## Mã giả:

```python
Khởi tạo quần thể có N kiểu gen
Tính độ thích nghi (fitness) của từng cá thể
while (chưa đạt điều kiện dừng) do:
    Chọn các cá thể từ quần thể hiện tại làm cá thể cha mẹ
    while (chưa đạt N cá thể con) do:
        Tiến hành lai ghép (crossover) với xác suất p_c
        Tiến hành đột biến (mutation) cá thể con với xác suất p_m
    Sắp xếp không trội
    Tính khoảng cách mật độ
    Chọn lọc sinh tồn
```

## Các kiểu mã hóa

## Các cách chọn lọc cha mẹ

## Các kiểu lai ghép

## Các kiểu đột biến

## Chọn lọc sinh tồn

### Mã giả

```bash

```
