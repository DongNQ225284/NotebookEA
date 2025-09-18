## Mục lục

- [Giới thiệu về Thuật toán Genetic Algorithm - GA](#giới-thiệu-về-thuật-toán-genetic-algorithm---ga)
- [Mã giả](#mã-giả)
- [Các kiểu mã hóa](#các-kiểu-mã-hóa)
  - [Mã hóa nhị phân](#mã-hóa-nhị-phân)
  - [Mã hóa hoán vị](#mã-hóa-hoán-vị)
  - [Mã hóa kề](#mã-hóa-kề)
  - [Mã hóa số thực](#mã-hóa-số-thực)
- [Các phương pháp chọn lọc cá thể cha mẹ](#các-phương-pháp-chọn-lọc-cá-thể-cha-mẹ)
  - [Random Selection (RS)](#chọn-lọc-ngẫu-nhiên---random-selection---rs)
  - [Fitness Proportion Selection (FPS)](#chọn-lọc-theo-giá-trị-thích-nghi---fitness-proportion-selection---fps)
  - [Độ thích nghi tiêu chuẩn](#độ-thích-nghi-tiêu-chuẩn)
  - [Độ thích nghi xếp hạng](#độ-thích-nghi-xếp-hạng)
  - [Độ thích nghi xếp hạng dựa trên khoảng cách phân ly](#độ-thích-nghi-xếp-hạng-dựa-trên-khoảng-cách-phân-ly)
- [Các kiểu lai ghép (Crossover)](#các-kiểu-lai-ghép-crossover)
  - [Lai ghép với mã hóa nhị phân](#lai-ghép-với-mã-hóa-nhị-phân)
  - [Lai ghép với mã hóa hoán vị](#lai-ghép-với-mã-hóa-hoán-vị)
  - [Lai ghép với mã hóa số thực](#lai-ghép-với-mã-hóa-số-thực)
- [Các toán tử đột biến (Mutation)](#các-toán-tử-đột-biến-mutation)
  - [Đột biến đảo bit](#đột-biến-đảo-bit)
  - [Đột biến trao đổi chéo](#đột-biến-trao-đổi-chéo-swap-mutation)
  - [Đột biến đảo đoạn](#đột-biến-đảo-đoạn-inversion-mutation)
  - [Đột biến trộn](#đột-biến-trộn-scramble-mutation)
  - [Đột biến đa thức](#đột-biến-đa-thức-polynomial-mutation)
- [Chọn lọc sinh tồn](#chọn-lọc-sinh-tồn)
  - [Chọn lọc xén](#chọn-lọc-xén-truncation-selection)
  - [Roulette Wheel Selection](#chọn-lọc-theo-vòng-quay-roulette-roulette-wheel-selection)
  - [Stochastic Universal Sampling](#chọn-lọc-theo-kiểu-dải-stochastic-universal-sampling)
  - [Chọn lọc cục bộ](#chọn-lọc-cục-bộ)
  - [Chọn lọc thứ tự](#chọn-lọc-thứ-tự)
  - [Chọn lọc giao đấu](#chọn-lọc-theo-giao-đấu)
- [Lựa chọn tham số](#lựa-chọn-tham-số)
- [Cài đặt thuật toán theo hướng đối tượng](#cài-đặt-thuật-toán-theo-hướng-đối-tượng)

# Giới thiệu về Thuật toán Genetic Algorithm - GA

Thuật toán tiến hóa là một thuật toán giải gần đúng, lấy cảm hứng từ quá trình tiến hóa của sinh vật trong thế giới tự nhiên

## Mã giả:

```bash
Khởi tạo quần thể có N kiểu gen
Tính độ thích nghi (fitness) của từng cá thể
while (chưa đạt điều kiện dừng) do:
    Chọn các cá thể từ quần thể hiện tại làm cá thể cha mẹ
    while (chưa đạt N cá thể con) do:
        Tiến hành lai ghép (crossover) với xác suất p_c
        Tiến hành đột biến (mutation) cá thể con với xác suất p_m
    Chọn lọc sinh tồn
```

## Các kiểu mã hóa.

### Mã hóa nhị phân

Ví dụ: Bài toán cái túi - **Knapsack Problem - KP**  
Cho n đồ vật với cân nặng và $w_i$ và giá trị sử dụng là $v_i$, biết không thể mang quá $capacity$, hãy tính giá trị lớn nhất có thể mang.

Mã hóa lời giải cho bài toán là một chuỗi bit, với $b_i$ là trạng thái tương ứng với đồ vật có giá trị $v_i$ và cân nặng $w_i$, với $1$ thể hiện mang theo và $0$ thì ngược lại.

| Đồ vật |  1  |  2  |  3  |  4  |  5  |
| :----: | :-: | :-: | :-: | :-: | :-: |
| $v_i$  |  1  |  4  |  4  |  3  |  8  |
| $w_i$  |  2  |  5  |  3  |  4  | 10  |
| $b_i$  |  1  |  0  |  1  |  1  |  0  |

Với ví dụ ở bảng trên, các đồ vật sẽ được đem theo là ${1, 3, 4}$

### Mã hóa hoán vị:

Ví dụ: Bài toán người đi du lịch **Traveling Salesman Problem - TSP**  
Một người từ thành phố $1$ muốn tham quan các thành phố $2, 3, .. n$ và quay trở lại thành phố ban đầu, biết chi phí giữa mỗi thành phố, tìm hành trình tốn ít chi phí nhất.

Mã hóa cho lời giải là một dãy số là hoán vị của $1, 2, 3, ... n$.

|  3  |  2  |  5  |  4  |
| :-: | :-: | :-: | :-: |

Với ví dụ bảng trên, hành trình sẽ là $1 \to 3 \to 2 \to 5 \to 4 \to 1$

### Mã hóa kề

Ví dụ: Bài toán người du lịch.  
Ta có một cách mã hóa khác, được mô tả như sau.  
Nếu hành trình di chuyển từ $i \to j$, thì vị trí thứ $i$ nhận giá trị $j$.  
Với hành trình $1 \to 3 \to 2 \to 5 \to 4 \to 1$ ta mã hóa như sau:

|  3  |  5  |  2  |  1  |  4  |
| :-: | :-: | :-: | :-: | :-: |

### Mã hóa số thực

Ví dụ: Tìm giá trị nhỏ nhất của hàm số $f(x, y, z) = xy + 3z^2 + 2$ trên tập $R$

Mã hóa lời giải là bộ ba $(x, y, z)$, giả sử$ (x, y, z) = (1.2, 3, 2.1)$ ta mã hóa như sau:

| 1.2 | 3.0 | 2.1 |
| :-: | :-: | :-: |

Tổng quát lại ta có thể thấy các cách mã hóa ở trên có một điểm chung là một dãy số, trong lập trình ta sẽ dùng **vector** để biểu diễn kiểu gen.

Chúng ta có thể sử dụng mã hóa số thực để mã hóa cho tất cả các kiểu trên

Ví dụ với mã hóa nhị phân, ta lấy ngẫu nhiên một số thực $[0, 0.5)$ để biểu thị $0$ và một số thực ngẫu nhiên $[0.5, 1]$ để biểu thị $1$

| Đồ vật  |  1  |  2  |  3  |  4  |  5  |
| :-----: | :-: | :-: | :-: | :-: | :-: |
|  $v_i$  |  1  |  4  |  4  |  3  |  8  |
|  $w_i$  |  2  |  5  |  3  |  4  | 10  |
|  $b_i$  |  1  |  0  |  1  |  1  |  0  |
| $Real $ | 0.6 | 0.2 | 0.7 | 0.9 | 0.1 |

Với kiểu mã hóa này, ta sẽ đơn giản hơn trong việc thực hiện việc chọn lọc quần thể, thực hiện lai ghép (crossover) và thực hiện đột biến cá thể (mutation). Tuy nhiên, với việc mã hóa này thì ta cần một hàm giải mã **decode**

## Các phương pháp chọn lọc cá thể cha mẹ

### Chọn lọc ngẫu nhiên - random selection - RS

Lấy ngẫu nhiên một số lượng cá thể trong tập quần thể (population) để bổ sung vào tập cha mẹ (listParent)

### Chọn lọc theo giá trị thích nghi - fitness proportion selection - FPS

#### Bánh xe Roulette:

Từ tập các giá trị thích nghi $(f_1, f_2, ..., f_n)$ chuyển sang tập giá trị xác suất $(p_1, p_2, ..., p_n)$ sau đó tiến hành chọn trên tập xác suất.

#### Chọn lọc giao đấu - tournament selection:

Chọn ngẫu nhiên $k$ cá thể từ quần thể, dựa vào độ thích nghi, chọn ra cá thể tốt nhất. Lặp lại cho tới khi đủ số lượng cha mẹ.

#### Chọn lọc thứ hạng - rank selection:

Sắp xếp lại giá trị thích nghi, với mỗi cá thể có một hạng ${1, 2, ..., n}$ chuyển sang tập xác suất rồi tiến hành chọn trên tập xác suất.

### Độ thích nghi tiêu chuẩn:

Với mỗi cá thể $i$ có một giá trị thích nghi $f_i$.  
Độ thích nghi tiêu chuẩn là $F_i = \frac{f_i}{\sum{f_i}}$

### Độ thích nghi xếp hạng:

Với mỗi cá thể $i$ sẽ có giá trị thích nghi $f_i$, hạng $j$.  
Độ thích nghi xếp hạng: $F_i = p\times(1 - p)^{j - 1}$, với $p \in [0, 1]$ được gọi là độ hút

### Độ thích nghi xếp hạng dựa trên khoảng cách phân ly:

Với mỗi cá thể $i$ sẽ có giá trị thích nghi $f_i$. Độ phân ly $P_i = \sum_{i \neq j}{\frac{1}{|f_i - f_j|}}$

Với mỗi cá thể $i$ có hạng $j$ theo giá trị thích nghi $f_i$ và hạng $k$ dựa trên độ phân ly $P_i$, khi đó cá thể này có tổng điểm là $L_i = j + k$, cá thể sẽ được xếp hạng $L_i$ dựa trên tổng điểm.
Khi đó cá thể sẽ có độ thích nghi dựa trên khoảng cách phân ly: $F_i = \frac{L_i}{\sum{L_i}}$

Các tập giá trị khi quy sang độ thích nghi tiêu chuẩn(xếp hạng) ta sẽ nhận được giá trị thuộc $[0, 1]$, như vậy trong quá trình chọn lọc sẽ khá giống với **chọn lọc Roulette**

## Các kiểu lai ghép (crossover)

### Lai ghép với mã hóa nhị phân

#### Lai ghép một điểm cắt (One-point crossover)

Ví dụ: Lai ghép một điểm cắt, chọn ra một điểm cắt để chia kiểu gen của cha mẹ thành 2 phần, sau đó hoán vị các đoạn gen của 2 cá thể cha mẹ cho nhau.  
Đầu vào:
| b | 0 | 0 | 0 | 1 | 1 | 1 | 1 | 1 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{p_1}$ | 1 | 0 | 0 | 0 | 1 | 0 | 1 | 0 |
| $\mathbf{p_2}$ | 0 | 1 | 1 | 1 | 0 | 1 | 0 | 0 |

Kết quả:
| b | 0 | 0 | 0 | 1 | 1 | 1 | 1 | 1 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c_1}$ | 1 | 0 | 0 | 1 | 0 | 1 | 0 | 0 |
| $\mathbf{c_2}$ | 0 | 1 | 1 | 0 | 1 | 0 | 1 | 0 |

#### Lai ghép đa điểm cắt

Ví dụ: Lai ghép 3 điểm cắt, chọn ra ba điểm cắt, theo thứ tự xen kẽ, hoán đổi các đoạn gen cha mẹ cho nhau.  
| b | 0 | 0 | 0 | 1 | 1 | 1 | 0 | 0 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{p_1}$ | 1 | 0 | 0 | 0 | 1 | 0 | 1 | 0 |
| $\mathbf{p_2}$ | 0 | 1 | 1 | 1 | 0 | 1 | 0 | 0 |

Kết quả:
| b | 0 | 0 | 0 | 1 | 1 | 1 | 0 | 0 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c_1}$ | 1 | 0 | 0 | 1 | 0 | 1 | 1 | 0 |
| $\mathbf{c_2}$ | 0 | 1 | 1 | 0 | 1 | 0 | 0 | 0 |

#### Lai ghép đồng nhất (uniform crossover)

Lai ghép đồng nhất hiểu đơn giản là từng phần tử trong kiểu gen có một xác xuất $p$ được hoán đổi cho nhau.  
Ví dụ:  
Đầu vào:
| b | 1 | 0 | 1 | 1 | 0 | 0 | 1 | 0 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{p_1}$ | 1 | 0 | 0 | 0 | 1 | 0 | 1 | 0 |
| $\mathbf{p_2}$ | 0 | 1 | 1 | 1 | 0 | 1 | 0 | 0 |

Kết quả:
| b | 1 | 0 | 1 | 1 | 0 | 0 | 1 | 0 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c_1}$ | 0 | 0 | 1 | 1 | 1 | 0 | 0 | 0 |
| $\mathbf{c_2}$ | 1 | 1 | 0 | 0 | 0 | 1 | 1 | 0 |

### Lai ghép với mã hóa hoán vị

#### Lai ghép theo thứ tự (order crossover)

Ví dụ:
Đầu vào:
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{p_1}$ | 2 | 1 | 5 | 3 | 7 | 6 | 4 | 8 |
| $\mathbf{p_1}$ | 4 | 2 | 3 | 7 | 1 | 5 | 8 | 6 |

B0. Khởi tạo $\mathbf{c_1}, \mathbf{c_2}$
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c_1}$ | | | | | | | | |
| $\mathbf{c_2}$ | | | | | | | | |

B1. Chọn ngẫu một đoạn idx, sao chép giá trị tại vị trí của $\mathbf{p_1}$ vào $\mathbf{c_1}$, sao chép giá trị tại các vị trí của $\mathbf{p_2}$ vào $\mathbf{c_2}$
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{p_1}$ | 2 | 1 | 5 | 3 | 7 | 6 | 4 | 8 |
| $\mathbf{c_1}$ | | | 5 | 3 | 7 | 6 | | |
| $\mathbf{p_2}$ | 4 | 2 | 3 | 7 | 1 | 5 | 8 | 6 |
| $\mathbf{c_2}$ | | | 3 | 7 | 1 | 5 | | |

B2. Xác định thứ tự tính từ vị trí cắt thứ 2 là $(7)$ của $\mathbf{p_2}$, $\mathbf{p_1}$
| idx | 7 | 8 | 1 | 2 | 3 | 4 | 5 | 6 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{p_2}$ | 8 | 6 | 4 | 2 | 3 | 7 | 1 | 5 |
| $\mathbf{p_1}$ | 4 | 8 | 2 | 1 | 5 | 3 | 7 | 6 |

B3. Loại bỏ các phẩn tử của $\mathbf{p_2}$ đã xuất hiện trong $\mathbf{c_1}$ và loại bỏ các phần tử của $\mathbf{p_1}$ đã xuất hiện trong $\mathbf{c_2}$
| idx | 7 | 8 | 1 | 2 | 3 | 4 | 5 | 6 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{p_2}$ | 8 | | 4 | 2 | | | 1 | |
| $\mathbf{p_1}$ | 4 | 8 | 2 | | | | | 6 |

B4. Ghép chuỗi còn lại của $\mathbf{p_2}$ bắt đầu từ vị trí cắt thứ 2 $(7)$ vào $\mathbf{c_1}$ và của $\mathbf{p_1}$ vào $\mathbf{c_2}$
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c_1}$ | 2 | 1 | 5 | 3 | 7 | 6 | 8 | 4 |
| $\mathbf{c_2}$ | 2 | 6 | 3 | 7 | 1 | 5 | 4 | 8 |

Kết quả:
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c_1}$ | 2 | 1 | 5 | 3 | 7 | 6 | 8 | 4 |
| $\mathbf{c_2}$ | 2 | 6 | 3 | 7 | 1 | 5 | 4 | 8 |

#### Lai ghép ánh xạ từng phân (Partially Mapped Crossover)

Ví dụ:
Đầu vào:
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{p_1}$ | 2 | 1 | 5 | 3 | 7 | 6 | 4 | 8 |
| $\mathbf{p_2}$ | 4 | 2 | 3 | 7 | 1 | 5 | 8 | 6 |

B0. Khởi tạo $\mathbf{c_1}$, $\mathbf{c_2}$
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c_1}$ | | | | | | | | |
| $\mathbf{c_2}$ | | | | | | | | |

B1. Chọn ngẫu một đoạn idx, sao chép giá trị tại vị trí của $\mathbf{p_1}$ vào $\mathbf{c_1}$, sao chép giá trị tại các vị trí của $\mathbf{p_2}$ vào $\mathbf{c_2}$
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{p_1}$ | 2 | 1 | 5 | 3 | 7 | 6 | 4 | 8 |
| $\mathbf{c_1}$ | | | 5 | 3 | 7 | 6 | | |
| $\mathbf{p_2}$ | 4 | 2 | 3 | 7 | 1 | 5 | 8 | 6 |
| $\mathbf{c_2}$ | | | 3 | 7 | 1 | 5 | | |

B2. Điền các gen còn thiếu từ trái qua phải, nếu như gen này đã tồn tại, thực hiện phép ánh xạ.
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{p_1}$ | 2 | 1 | 5 | 3 | 7 | 6 | 4 | 8 |
| $\mathbf{c_1}$ | 4 | 2 | 5 | 3 | 7 | 6 | 8 | **1** |
| $\mathbf{p_2}$ | 4 | 2 | 3 | 7 | 1 | 5 | 8 | 6 |
| $\mathbf{c_2}$ | 2 | **6** | 3 | 7 | 1 | 5 | 4 | 8 |

Với $\mathbf{c_1}$. Ở vị trí 8, vì $\mathbf{p_2}[8] = 6$ (đã tồn tại)  
Thực hiện ánh xạ $\mathbf{p_1}$ $\leftrightarrow$ $\mathbf{p_2}$:  
Thực hiện ánh xạ $6 \leftrightarrow 5$ (đã tồn tại)  
Thực hiện ánh xạ $5 \leftrightarrow 3$ (đã tồn tại)  
Thực hiện ánh xạ $3 \leftrightarrow 7$ (đã tồn tại)  
Thực hiện ánh xạ $7 \leftrightarrow 1$ (chưa tồn tại)  
Nên $\mathbf{c_1}[8] = 1$

Với $\mathbf{c_2}$.  
Ở vị trí thứ 2, vì $\mathbf{p_1}[2] = 1$ (đã tồn tại)  
Thực hiện ánh xạ $\mathbf{p_2}$ $\leftrightarrow$ $\mathbf{p_1}$:  
Thực hiện ánh xạ $1 \leftrightarrow 7$ (đã tồn tại)  
Thực hiện ánh xạ $7 \leftrightarrow 3$ (đã tồn tại)  
Thực hiện ánh xạ $3 \leftrightarrow 5$ (đã tồn tại)  
Thực hiện ánh xạ $5 \leftrightarrow 6$ (chưa tồn tại)  
Nên $\mathbf{c_2}[2] = 6$

Kết quả:
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c_1}$ | 4 | 2 | 5 | 3 | 7 | 6 | 8 | 1 |
| $\mathbf{c_2}$ | 2 | 6 | 3 | 7 | 1 | 5 | 4 | 8 |

#### Lai ghép chu trình (Cycle crossover)

Ví dụ:
Đầu vào:
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{p_1}$ | 2 | 1 | 5 | 3 | 7 | 6 | 4 | 8 |
| $\mathbf{p_2}$ | 4 | 3 | 2 | 7 | 1 | 5 | 8 | 6 |

B0. Khởi tạo $\mathbf{c_1}$, $\mathbf{c_2}$
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c_1}$ | | | | | | | | |
| $\mathbf{c_2}$ | | | | | | | | |

### Lai ghép với mã hóa số thực

#### Lai ghép số thực đơn giản

Ví dụ:
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{p_1}$ |0.1|0.4|0.1|0.8|0.2|0.3|0.7|0.5|
| $\mathbf{p_2}$ |0.2|0.3|0.4|0.6|0.5|0.7|0.8|0.2|

B0. Chọn ngẫu nhiên một vị trí lai ghép $k = 5$, chọn ngẫu nhiên $\alpha = 0.7 \in [0, 1]$
Khởi tạo $\mathbf{c_1}$, $\mathbf{c_2}$
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c_1}$ | | | | | | | | |
| $\mathbf{c_2}$ | | | | | | | | |

B1. Lấy các phần tử từ $1 \to k$ sao chép $\mathbf{p_1}$ vào $\mathbf{c_1}$ và $\mathbf{p_2}$ vào $\mathbf{c_2}$
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c_1}$ |0.1|0.4|0.1|0.8|0.2| | | |
| $\mathbf{c_2}$ |0.2|0.3|0.4|0.6|0.5| | | |

B2. Các phần tử còn lại được tính theo công thức:  
$\mathbf{c_1}[i] = \alpha \cdot \mathbf{p_1}[i] + (1 - \alpha)\cdot \mathbf{p_2}[i] $  
$\mathbf{c_2}[i] = \alpha \cdot \mathbf{p_2}[i] + (1 - \alpha)\cdot \mathbf{p_1}[i] $

|      idx       |  1  |  2  |  3  |  4  |  5  |  6   |  7   |  8   |
| :------------: | :-: | :-: | :-: | :-: | :-: | :--: | :--: | :--: |
| $\mathbf{c_1}$ | 0.1 | 0.4 | 0.1 | 0.8 | 0.2 | 0.42 | 0.73 | 0.41 |
| $\mathbf{c_2}$ | 0.2 | 0.3 | 0.4 | 0.6 | 0.5 | 0.58 | 0.85 | 0.29 |

Kết quả:
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:--:|:--:|:--:|
| $\mathbf{c_1}$ |0.1|0.4|0.1|0.8|0.2|0.42|0.73|0.41|
| $\mathbf{c_2}$ |0.2|0.3|0.4|0.6|0.5|0.58|0.85|0.29|

#### Lai ghép mô phỏng nhị phân (Simulated Binary Crossover)

B0. Lấy ngẫu nhiên $r \in [0, 1]$  
Tính $\beta$  
$\beta = (2r)^\frac{1}{\eta_c + 1}, r \leq 0.5$  
$\beta = (\frac{1}{2(1 - r)})^\frac{1}{\eta_c + 1}, r > 0.5$

B1. Khi đó các cá thể con được tính theo công thức:  
$\mathbf{c_1} = 0.5 \cdot ((1 + \beta) \cdot \mathbf{p_1} + (1 - \beta) \cdot \mathbf{p_2}) $  
$\mathbf{c_2} = 0.5 \cdot ((1 + \beta) \cdot \mathbf{p_2} + (1 - \beta) \cdot \mathbf{p_1}) $

## Các toán tử đột biến (Mutation)

### Đột biến đảo bit

Áp dụng cho mã hóa bit
Ví dụ:
Đầu vào:
| $\mathbf{c}$ | 1 | 0 | 0 | 0 | 1 | 0 | 1 | 0 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|

Xác suất đột biến $p_m = 0.4$

B1. Với mỗi gen, lấy một giá trị $r \in [0, 1]$
| r |0.4|0.2|0.1|0.9|0.6|0.5|0.3|0.2|
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c}$ | 1 | 0 | 0 | 0 | 1 | 0 | 1 | 0 |

B2. Nếu $r_i$ $\leq$ $p_m$ thì đảo bit
| r |0.4|0.2|0.1|0.9|0.6|0.5|0.3|0.2|
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c}$ | 0 | 1 | 1 | 0 | 1 | 0 | 0 | 1 |

Kết quả:
| $\mathbf{c}$ | 0 | 1 | 1 | 0 | 1 | 0 | 0 | 1 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|

### Đột biến trao đổi chéo (Swap mutation)

Thường áp dụng cho mã hóa hoán vị.  
Ví dụ:  
Đầu vào:
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c}$ | 1 | 3 | 4 | 7 | 2 | 8 | 6 | 5 |

B1. Chọn ra 2 vị trí khác nhau bất kỳ $p_1 = 2, p_2 = 5$
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c}$ | 1 | **3** | 4 | 7 | **2** | 8 | 6 | 5 |

B2. Swap($\mathbf{c}[p_1], \mathbf{c}[p_2]$)
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c}$ | 1 | **2** | 4 | 7 | **3** | 8 | 6 | 5 |

Kết quả:
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c}$ | 1 | **2** | 4 | 7 | **3** | 8 | 6 | 5 |

### Đột biến đảo đoạn (Inversion mutation)

Thường áp dụng cho mã hóa hoán vị.  
Ví dụ:  
Đầu vào:
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c}$ | 1 | 3 | 4 | 7 | 2 | 8 | 6 | 5 |

B1. Chọn ra một đoạn $[p_1, p_2]$ bất kỳ giả sử $p_1 = 2, p_2 = 5$
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c}$ | 1 | **3** | **4** | **7** | **2** | 8 | 6 | 5 |

B2. Đảo ngược đoạn vừa chọn
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c}$ | 1 | **2** | **7** | **4** | **3** | 8 | 6 | 5 |

Kết quả:
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c}$ | 1 | **2** | **7** | **4** | **3** | 8 | 6 | 5 |

### Đột biến trộn (Scramble mutation)

Thường áp dụng cho mã hóa hoán vị  
Ví dụ:  
Đầu vào:
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c}$ | 1 | 3 | 4 | 7 | 2 | 8 | 6 | 5 |

B1. Chọn ra một đoạn $[p_1, p_2]$ bất kỳ giả sử $p_1 = 2, p_2 = 5$
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c}$ | 1 | **3** | **4** | **7** | **2** | 8 | 6 | 5 |

B2. Xếp các vị trí chẵn và lẻ liền nhau
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c}$ | 1 | **3** | **7** | **4** | **2** | 8 | 6 | 5 |

Kết quả:
| idx | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| $\mathbf{c}$ | 1 | **3** | **7** | **4** | **2** | 8 | 6 | 5 |

### Đột biến đa thức (Polynomial mutation)

Thường áp dụng cho mã hóa số thực  
Đầu vào: $\mathbf{x}$  
B1. Lấy ngẫu nhiên $\mu \in [0, 1]$

B2. Tại mỗi vị trí $i$ tính các giá trị $\delta^L$ và $\delta^R$  
$\delta^L = (2\mu)^{\frac{1}{1 + \eta_m}} - 1$ với $\mu \leq 0.5$  
$\delta^R = 1 - (2 - 2\mu)^{\frac{1}{1 + \eta_m}}$ với $\mu > 0.5$

B3. Cập nhật giá trị theo công thức  
$\mathbf{c}[i] = \mathbf{x}[i] + \delta^L \cdot \mathbf{x}[i]$ với $\mu \leq 0.5$  
$\mathbf{c}[i] = \mathbf{x}[i] + \delta^R \cdot (1 - \mathbf{x}[i])$ với $\mu > 0.5$

Kết quả: $\mathbf{c}$

## Chọn lọc sinh tồn

### Chọn lọc xén (Truncation selection)

Trong bước chọn lọc, các cá thể sẽ được sắp xếp giảm dần về giá trị thích nghi, một ngưỡng Trunc được đưa ra để quyết định xem những cá thể nào được chọn  
Ví dụ:
| STT | fitness |
|:---:|:-------:|
| 1 | 0.5 |
| 2 | 0.3 |
| 3 | 0.2 |
| 4 | 0.1 |
| 5 | 0.05 |
| 6 | 0.024 |

Sau bước chọn lọc với $Trunc = 0.50 $
| STT | fitness |
|:---:|:-------:|
| 1 | 0.5 |
| 2 | 0.3 |
| 3 | 0.2 |

Sau bước chọn lọc với $Trunc = 0.35 $
| STT | fitness |
|:---:|:-------:|
| 1 | 0.5 |
| 2 | 0.3 |

### Chọn lọc theo vòng quay Roulette (Roulette wheel selection)

Đây là một kiểu chọn lọc theo xác suất theo giá trị thích nghi, giá trị thích nghi càng cao, thì sẽ xác suất lựa chọn càng cao.  
Đầu vào:  
Quần thể, $N$ cá thể cho thế hệ tiếp theo.  
B1. Với cá thể thứ $i$ có giá trị thích nghi $f_i$ và độ thích nghi tiêu chuẩn $F_i$ với $F_i \in [0, 1]$ và $\sum{F_i} = 1$

B2. Xếp các cá thể lên một dải  
| $F$ | $F_1$ | $F_2$ | ... | $F_i$ | ... | $F_N$ |
|:---:|:-----:|:-----:|:---:|:-----:|:---:|:-----:|
| $S$ | $S_1$ | $S_2$ | ... | $S_i$ | ... | $S_N$ |

Với $S_n = \sum_{i = 1}^n{F_i}$

B3. Chọn một số ngẫu nhiên $r \in [0, 1]$. Tìm vị trí $i$ thỏa mãn: $S_{i - 1} < r \leq S_i$

Kết quả: Cá thể $i$ là cá thể được lựa chọn.

### Chọn lọc theo kiểu dải (Stochastic universal sampling)

Ý tưởng gần giống với chọn lọc vòng quay Roulette, tuy nhiên với mỗi lần lấy ngẫu nhiên có thể chọn ra nhiều hơn một cá thể.

B1. Với cá thể thứ $i$ có giá trị thích nghi $f_i$ và độ thích nghi tiêu chuẩn $F_i$ với $F_i \in [0, 1]$ và $\sum{F_i} = 1$

B2. Xếp các cá thể lên một dải  
| $F$ | $F_1$ | $F_2$ | ... | $F_i$ | ... | $F_N$ |
|:---:|:-----:|:-----:|:---:|:-----:|:---:|:-----:|
| $S$ | $S_1$ | $S_2$ | ... | $S_i$ | ... | $S_N$ |

Với $S_n = \sum_{i = 1}^n{F_i}$

B3. Chọn một số ngẫu nhiên $r_1 \in [0, 1]$.
Với $L$ cá thể cần chọn, xác định $r_1, $r_2, ..., r_L$ theo công thức

$r_i = r_{i - 1} + \frac{1}{L}$ nếu $r_{i - 1} + \frac{1}{L} < 1$  
$r_i = r_{i + 1} + \frac{1}{L} - 1$ nếu $r_{i - 1} + \frac{1}{L} > 1$

B4. Với mỗi $r_j$ Tìm vị trí $i$ thỏa mãn: $S_{i - 1} < r_j \leq S_i$. Khi đó cá thể $i$ là cá thể được chọn với $r_j$

Kết quả: Tập các cá thể được chọn

### Chọn lọc cục bộ

Chọn ra một cá thể trong quần thể, sau đó chọn các cá thể nằm lân cận. Với mỗi bài toán, cần định nghĩa thế nào là lân cận?

### Chọn lọc thứ tự

##### Chọn lọc truyến tính (linear selection)

B1. Sắp xếp các cá thể trong quần thể theo thứ tự tăng gần về giá trị thích nghi.  
B2. Với cá thể thứ $i$ có giá trị thích nghi $f_i$ và có rank $k$. Giá trị xếp hạng của cá thể này là $F_i$  
Với $F_i = 2 - P + 2 \times (P - 1) \times \frac{k - 1}{N - 1}$, với $P$ là hệ sống phóng đại $P \in [1.0, 2.0]$

B3. Thực hiện chọn lọc Roulette với tập giá trị vừa thu được

##### Chọn lọc cắt xén

Ý tưởng là sẽ nhân bản những cá thể tốt nhất trong quần thể. Chọn ra $N / k$ cá thể từ quần thể, với mỗi cá thể nhân bản $k$ lần.

### Chọn lọc theo giao đấu

Chọn ra $k$ cá thể ngẫu nhiên trong tập quần thể, cá thể tốt nhất được lựa chọn, lặp lại cho tới khi đủ số lượng

## Lựa chọn tham số

# Cài đặt thuật toán theo hướng đối tượng

Trong thư mục này có các file minh họa cho việc triển khai GA dưới dạng hướng đối tượng (OOP):

```bash
GA
├── GA.ipynb: Triển khai tổng quát thuật toán GA (khung class chung).
├── function.ipynb: Triển khai GA cho bài toán tìm giá trị nhỏ nhất của một hàm số.
├── KP.ipynb: Triển khai GA cho bài toán Cái túi (Knapsack Problem).
├── TSP.ipynb: Triển khai GA cho bài toán Người đi du lịch (Traveling Salesman Problem).
├── Rosenbrock.ipynb: Triển khai GA cho bài toán tối ưu hàm Rosenbrock.
└── SNR.ipynb: Ứng dụng GA cho bài toán tối ưu SNR.
```

> Mỗi notebook đều kế thừa từ khung class GA tổng quát và tùy biến lại hàm **fitness** cũng như các toán tử (crossover, mutation) phù hợp với từng bài toán.
