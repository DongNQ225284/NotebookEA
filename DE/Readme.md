# MỤC LỤC TỔNG HỢP (CÓ LIÊN KẾT)

- [PHẦN 1: Thuật toán Tiến hóa Vi phân (DE)](#phần-1-thuật-toán-differential-evolution)

  - [1.1. Giới thiệu](#11-giới-thiệu-về-thuật-toán-de)
  - [1.2. Cá thể trong DE](#12-cá-thể-trong-de)
  - [1.3. Mã giả tổng quát](#13-mã-giả-tổng-quát)
  - [1.4. Mã hóa trong DE](#14-các-kiểu-mã-hóa)
  - [1.5. Chọn cá thể để đột biến](#15-các-phương-pháp-chọn-lọc-cá-thể-để-tạo-đột-biến)
  - [1.6. Toán tử đột biến](#16-các-toán-tử-đột-biến-mutation-operators)
  - [1.7. Toán tử lai ghép](#17-các-kiểu-lai-ghép-crossover)
  - [1.8. Chọn lọc sinh tồn](#18-chọn-lọc-sinh-tồn-survival-selection)
  - [1.9. Tham số đầu vào](#19-tham-số-đầu-vào)

- [PHẦN 2: Thuật toán SHADE](#phần-2-thuật-toán-shade-success-history-based-adaptive-de)
  - [2.1. Bối cảnh & vấn đề của DE](#21-bối-cảnh--vấn-đề-của-de)
  - [2.2. Cải tiến trong SHADE](#22-điểm-khác-biệt-và-cải-tiến-trong-shade)
- [PHẦN 3: Thuật toán L-SHADE](#phần-3-thuật-toán-l-shade-linear-population-size-reduction-shade)
  - [3.1. Bối cảnh & vấn đề của SHADE](#31-bối-cảnh--vấn-đề-của-shade)
  - [3.2. Cải tiến trong L-SHADE](#32-điểm-khác-biệt-và-cải-tiến-trong-l-shade)

---

# PHẦN 1: THUẬT TOÁN DIFFERENTIAL EVOLUTION

## 1.1. Giới thiệu về Thuật toán DE

**Differential Evolution (DE)** là một thuật toán tối ưu hóa dựa trên quần thể, được thiết kế để giải các bài toán tối ưu hóa trên miền số thực liên tục.  
Không giống như **GA (Genetic Algorithm)** vốn phù hợp với chuỗi nhị phân hoặc rời rạc, DE làm việc trực tiếp với **vector số thực**.

---

## 1.2. Cá thể trong DE

Mỗi cá thể (Individual) trong DE là một vector thực $D$ chiều:

- **Vector vị trí:**  
  $$x = (x_1, x_2, \ldots, x_D)$$
- **Fitness:** Giá trị hàm mục tiêu $f(x)$.

---

## 1.3. Mã giả tổng quát

```bash
Khởi tạo quần thể với N cá thể ngẫu nhiên
Đánh giá fitness cho từng cá thể

While (chưa đạt điều kiện dừng):
    For i = 1 to N:
        Đột biến (Mutation): Tạo vector v từ 3 cá thể khác
        Lai ghép (Crossover): Tạo vector u từ x_i và v
        Chọn lọc (Selection): So sánh u và x_i, giữ lại người giỏi hơn
```

## 1.4. Các kiểu mã hóa

DE sử dụng **Mã hóa số thực (Real-valued Encoding)** do đặc thù của bài toán cần làm việc trực tiếp với các tham số dạng số thực.

**Ví dụ:** Bài toán tìm kích thước 3 cạnh của một hộp chữ nhật tối ưu.

| Cạnh 1 ($x_1$) | Cạnh 2 ($x_2$) | Cạnh 3 ($x_3$) |
| :------------: | :------------: | :------------: |
|      10.5      |      5.2       |      8.1       |

---

## 1.5. Các phương pháp chọn lọc cá thể để tạo đột biến

Trong DE, bước chọn cá thể để đột biến được tích hợp trong toán tử **Mutation**.

Không giống GA sử dụng chọn lọc theo độ thích nghi (như Roulette Wheel), DE **chọn ngẫu nhiên hoàn toàn** các cá thể $r1, r2, r3$ từ quần thể để đảm bảo tính đa dạng và tạo ra nhiễu vi phân.

---

## 1.6. Các toán tử đột biến (Mutation Operators)

Toán tử đột biến là **trung tâm** của DE. Từ cá thể gốc $x_i$, ta tạo vector đột biến $v_i$.

### 🔹 Chiến lược DE/rand/1 (cổ điển)

Chọn ngẫu nhiên 3 cá thể khác nhau $r1, r2, r3$ (và khác $i$):

$$
v_i = x_{r1} + F \cdot (x_{r2} - x_{r3})
$$

- $F$ (Scaling Factor) điều chỉnh mức độ đột biến, thường $F \in [0, 2]$.

### 🔹 Chiến lược DE/best/1

Dùng cá thể tốt nhất của quần thể:

$$
v_i = x_{best} + F \cdot (x_{r1} - x_{r2})
$$

---

## 1.7. Các kiểu lai ghép (Crossover)

Sau khi có vector đột biến $v_i$, ta lai ghép nó với vector hiện tại $x_i$ để tạo vector thử nghiệm $u_i$.

### 🔹 Lai ghép nhị thức (Binomial Crossover)

Với từng gen $j$:

- Sinh số ngẫu nhiên $rand \in [0,1]$
- Nếu $rand \le Cr$: nhận gen từ $v_i$
- Ngược lại: giữ gen từ $x_i$

Trong đó:

- $Cr$ là xác suất lai ghép, $Cr \in [0,1]$

---

## 1.8. Chọn lọc sinh tồn (Survival Selection)

DE dùng cơ chế **Greedy Selection** theo cặp 1–1:

- Nếu $f(u_i)$ tốt hơn hoặc bằng $f(x_i)$ → chọn $u_i$
- Ngược lại → giữ $x_i$

---

## 1.9. Tham số đầu vào

Người dùng phải chọn cố định:

- **Kích thước quần thể:** $N$
- **Hệ số tỉ lệ:** $F$
- **Xác suất lai ghép:** $Cr$

---

# PHẦN 2: THUẬT TOÁN SHADE (SUCCESS-HISTORY BASED ADAPTIVE DE)

## 2.1. Bối cảnh & Vấn đề của DE

Trong DE truyền thống (Phần 1), các tham số $F$ và $Cr$ phải được chọn **cứng** ngay từ đầu.

- Ví dụ: $F = 0.5$, $Cr = 0.9$
- Nhưng **mỗi bài toán** lại phù hợp với một bộ tham số khác nhau.
- Thậm chí trong **cùng một bài toán**, ta cần tham số khác nhau theo từng giai đoạn:
  - Giai đoạn đầu → cần **khám phá rộng** → $F$ lớn
  - Giai đoạn sau → cần **tinh chỉnh** → $F$ nhỏ

❗ Việc chọn sai tham số có thể khiến DE:

- Hội tụ chậm
- Hoặc **không tìm được nghiệm tốt**

---

## 2.2. Điểm khác biệt và Cải tiến trong SHADE

SHADE giải quyết câu hỏi:  
**“Làm sao để không phải mò mẫm chọn tham số?”**  
→ Bằng cách dùng **cơ chế tự thích nghi dựa trên lịch sử thành công (Success-History).**

---

### 1. Cơ chế thích nghi tham số (Adaptive Parameter)

Thay vì dùng giá trị cố định cho cả quần thể, SHADE sinh ra **cặp tham số riêng ($F_i$, $Cr_i$)** cho từng cá thể, dựa trên **Bộ nhớ lịch sử (Memory M)**.

Cách hoạt động:

1. Với mỗi cá thể con $u$ **tốt hơn** cá thể cha $x$,  
   → bộ tham số $(F_i, Cr_i)$ đã tạo ra nó được xem là **thành công**.

2. Cuối mỗi thế hệ, SHADE tính **trung bình trọng số** của các tham số thành công này.

3. Kết quả được **lưu vào bộ nhớ**, để các thế hệ sau học theo.

➡️ Thuật toán tự điều chỉnh tham số theo hướng **hiệu quả nhất**, không cần con người chọn thủ công.

---

### 2. Chiến lược đột biến mới: current-to-pbest/1

SHADE không dùng DE/rand/1 (quá ngẫu nhiên) hoặc DE/best/1 (dễ mắc kẹt cục bộ).  
Nó sử dụng chiến lược lai:

$$
v_i = x_{i} + F_i \cdot (x_{pbest} - x_{i}) + F_i \cdot (x_{r1} - x_{r2}')
$$

Trong đó:

- **$x_{pbest}$**: chọn từ _top $p\%$ cá thể tốt nhất_.  
  → Cân bằng giữa định hướng tốt và duy trì đa dạng.

- **$x_{r2}'$**: lấy từ _(Quần thể hiện tại + Archive)_  
  → **Archive** chứa các cá thể _kém_ vừa bị loại bỏ ở bước chọn lọc trước.  
  → Giúp giữ sự đa dạng gen và tránh hội tụ sớm.

- **$r1, r2'$**: được chọn ngẫu nhiên, độc lập và khác với $i$.

✔ Đây là cải tiến quan trọng giúp SHADE trở thành một trong những phiên bản DE mạnh nhất trước khi L-SHADE ra đời.

---

# PHẦN 3: THUẬT TOÁN L-SHADE (LINEAR POPULATION SIZE REDUCTION SHADE)

## 3.1. Bối cảnh & Vấn đề của SHADE

SHADE đã giải quyết tốt vấn đề **tự thích nghi tham số**, nhưng vẫn giữ **kích thước quần thể cố định** suốt quá trình chạy.

Ví dụ: $N = 100$ từ đầu đến cuối.

### Vấn đề phát sinh

- **Giai đoạn đầu:** Cần quần thể lớn để khám phá không gian tìm kiếm (exploration).
- **Giai đoạn cuối:** Khi quần thể đã tụ tập quanh vùng tối ưu, việc giữ nguyên 100 cá thể trở nên **tốn kém và không cần thiết**.

➡️ Điều này làm giảm hiệu suất tính toán, đặc biệt trong các bài toán tối ưu khó và giới hạn số lần đánh giá hàm (NFE).

---

## 3.2. Điểm khác biệt và Cải tiến trong L-SHADE

L-SHADE **kế thừa toàn bộ SHADE**, bao gồm:

- Bộ nhớ lịch sử tham số
- Archive lưu lại cá thể kém
- Mutation **current-to-pbest/1**

và bổ sung thêm một cơ chế rất mạnh:

---

### 1. Cơ chế LPSR (Linear Population Size Reduction)

L-SHADE **giảm kích thước quần thể $N$ theo cách tuyến tính** trong suốt quá trình tối ưu.

Mục tiêu:

- Đầu → quần thể lớn → khám phá mạnh
- Cuối → quần thể nhỏ → tinh chỉnh hiệu quả, giảm chi phí tính toán

Sau mỗi thế hệ, số lượng cá thể mới được tính bởi:

$$
N_{G+1} = \text{round}\left(
\left( \frac{N_{min} - N_{init}}{MAX\_NFE} \right) \cdot NFE + N_{init}
\right)
$$

Trong đó:

- $N_{init}$: kích thước quần thể ban đầu
- $N_{min}$: kích thước nhỏ nhất (thường là 4)
- $NFE$: số lần đánh giá hàm đã dùng
- $MAX\_NFE$: số lần đánh giá hàm tối đa cho phép

Ở mỗi thế hệ:

- Nếu $N_{G+1} < N_{G}$ → **xóa các cá thể có fitness kém nhất**
- Quần thể được cập nhật lại trước khi bước sang thế hệ tiếp theo

---

### 2. Tác động của cải tiến LPSR

#### ✔ **Tăng tốc tính toán**

Vì số cá thể giảm dần theo thời gian, chi phí cho:

- Đột biến
- Lai ghép
- Đánh giá hàm

đều giảm mạnh ở giai đoạn sau.

#### ✔ **Hiệu quả tối ưu hóa cao hơn**

- Giai đoạn đầu: được đầu tư nhiều tài nguyên để khám phá toàn cục
- Giai đoạn sau: tập trung vào tinh chỉnh trong vùng hẹp

➡️ Tối ưu hóa tài nguyên tính toán, nâng cao chất lượng nghiệm.

#### ✔ **Lý do L-SHADE trở thành một trong các thuật toán DE mạnh nhất**

Trong nhiều cuộc thi tối ưu hóa toàn cầu (CEC Competitions), L-SHADE thường xuyên **xếp hạng top đầu** nhờ:

- Khả năng thích nghi tham số (từ SHADE)
- Khả năng giảm dân số thông minh (LPSR)
- Duy trì tốt cân bằng giữa Exploration và Exploitation

---
