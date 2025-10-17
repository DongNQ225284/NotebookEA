# Giới thiệu

Notebook này trình bày về nhóm thuật toán tiến hóa để giải quyết các bài toán tối ưu

### MỤC LỤC

- [Cơ sở lý thuyết](#cơ-sở-lý-thuyết)

  - [Bài toán tối ưu](#bài-toán-tối-ưu)
  - [Các khái niệm cơ bản về cực tiểu](#các-khái-niệm-cơ-bản-về-cực-tiểu)
  - [Phân loại các bài toán tối ưu](#phân-loại-các-bài-toán-tối-ưu)
    - [Bài toán tối ưu nguyên (Tối ưu rời rạc/Tổ hợp)](#bài-toán-tối-ưu-nguyên-tối-ưu-rời-rạctổ-hợp)
    - [Bài toán tối ưu liên tục](#bài-toán-tối-ưu-liên-tục)
    - [Bài toán tối ưu đa mục tiêu](#bài-toán-tối-ưu-đa-mục-tiêu)
  - [Các phương pháp giải bài toán tối ưu](#các-phương-pháp-giải-bài-toán-tối-ưu)

- [Giới thiệu về Tính toán Tiến hóa](#giới-thiệu-về-tính-toán-tiến-hóa)

## CƠ SỞ LÝ THUYẾT

### Bài toán tối ưu

**Bài toán tối ưu là gì?**

Bài toán tối ưu là việc tìm kiếm giá trị cực trị của một hàm số nào đó.

**Ví dụ:**

Tìm giá trị nhỏ nhất của hàm số $f(x) = x^2 + 1$ trên tập $D = [-1, 1]$. Lời giải cho bài toán này là tại **$x = 0$ hàm số $f(x)$ đạt giá trị cực tiểu bằng 1**.

**Phát biểu tổng quát:**

Cho một ánh xạ $f: D \to \mathbb{R}^n$ với $D \subseteq \mathbb{R}^n$.
Mục tiêu là $f(x) \to min$ hay tìm tập các giá trị $avgmin\{x \in D|f(x)\}$, với $x \in \mathbb{R}^n$.

**Các thành phần cơ bản:**

- **$f(x)$** được gọi là **hàm mục tiêu**.
- **$D$** là **không gian lời giải** hay **ràng buộc**. Không gian lời giải có thể phải thỏa mãn một số ràng buộc nào đó, có thể biểu diễn thông qua hàm ràng buộc $g(x)$.
- **Trạng thái** là tập các giá trị $avgmin\{x \in D│f(x)\}$.

---

### CÁC KHÁI NIỆM CƠ BẢN VỀ CỰC TIỂU

| Khái niệm                                           | Định nghĩa                                                                                                            | Phát biểu Toán học                                                                                                |
| :-------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------- |
| **Cực tiểu toàn cục** (Global minimum)              | Điểm tại đó $f(x)$ đạt giá trị nhỏ nhất trên tập $D$.                                                                 | $x_0$ được gọi là cực tiểu toàn cục, khi: $f(x_0) \le f(x) \forall x \in D$.                                      |
| **Cực tiểu toàn cục chặt** (Strict global minimum)  | Điểm tại đó $f(x)$ đạt giá trị nhỏ nhất và có tính trội, tức giá trị của nó nhỏ hơn hẳn các điểm hàng xóm xung quanh. | $x_0$ được gọi là cực tiểu toàn cục chặt khi: $f(x_0) < f(x) \forall x \in D \setminus \{x_0\}$.                  |
| **Cực tiểu địa phương** (local optimization)        | Là điểm cực tiểu của hàm số $f(x)$.                                                                                   | $x_0$ được gọi là cực tiểu toàn cục khi: $f(x_0) \le f(x) \forall x \in [x_0 - \epsilon, x_0 + \epsilon] \cap D$. |
| **Cực tiểu địa phương chặt** (Strict local optimum) | Là điểm cực tiểu và có tính trội.                                                                                     | $f(x_0) < f(x) \forall x \in [x_0 - \epsilon) \cup (x_0 + \epsilon] \cap D$.                                      |

**Đánh giá tính "chặt":**

- Để đánh giá một điểm có "chặt" hay không về mặt toán học, ta đánh giá như sau: Điểm $x_0$ được gọi là chặt nếu **$f''(x_0) > 0$**.

---

### PHÂN LOẠI CÁC BÀI TOÁN TỐI ƯU

Tùy thuộc vào hàm mục tiêu và hàm ràng buộc, bài toán tối ưu được phân loại thành các dạng sau:

- **Quy hoạch tuyến tính:** Nếu hàm mục tiêu là tuyến tính và không gian lời giải là một tập lồi.
- **Quy hoạch phi tuyến:** Nếu hàm mục tiêu hoặc hàm ràng buộc **không phải tuyến tính** (afin).
- **Quy hoạch nguyên:** Nếu không gian lời giải là các giá trị **nguyên rời rạc**.
- **Quy hoạch đa mục tiêu:** Nếu có nhiều hơn một hàm mục tiêu cần tối ưu.
- **Quy hoạch ngẫu nhiên:** Khi hàm mục tiêu hoặc hàm ràng buộc chứa các hệ số không rõ ràng, mà giá trị được tuân theo một phân phối nào đó.
- **Quy hoạch tham số:** Khi hàm mục tiêu hoặc hàm ràng buộc chứa các tham số.

#### Bài toán tối ưu nguyên (Tối ưu rời rạc/Tổ hợp)

- **Đặc điểm:** Tập lời giải là các giá trị rời rạc.
- **Phát biểu toán học:** $I = (U, P, f, extr)$.
  - $U$: Không gian lời giải.
  - $P$: Các ràng buộc.
  - $f$: Hàm mục tiêu cần tối ưu.
  - $extr$: Cực trị (thường là cực đại hoặc cực tiểu).
- **Ví dụ thường gặp:** Bài toán người du lịch (TSP), Bài toán cái túi (KP), Bài toán phân công (AP), Bài toán cây khung nhỏ nhất (MST).

#### Bài toán tối ưu liên tục

- **Đặc điểm:** Tập lời giải là các giá trị liên tục.
- **Phát biểu toán học:** $\min_{x \in \mathbb{R}^n} f(x)$ với $g_i(x) \le 0$.

#### Bài toán tối ưu đa mục tiêu

- **Đặc điểm:** Khi có nhiều hơn hai mục tiêu cần tối ưu. Thường không thể tìm được một điểm $x$ cho tất cả các hàm cùng đạt giá trị nhỏ nhất.
- **Khái niệm Tối ưu Pareto:**

  - Một điểm **$x_1$ được gọi là tối ưu hơn $x_2$** nếu giá trị ở mọi hàm mục tiêu ở điểm $x_1$ không tồi hơn giá trị ở mọi hàm mục tiêu ở điểm $x_2$ và tồn tại một hàm mục tiêu có giá trị tối ưu tốt hơn.
  - **Điều kiện toán học:** $f_i(x_1) \le f_i(x_2) \forall i \text{ and } \exists i, j: f_i(x_1) < f_j(x_2)$
  - $x_1$ chi phối $x_2$ (ký hiệu toán học là $x_1 \prec x_2$).
  - **Biên Pareto:** Tập các điểm tối ưu (điểm được gọi là tối ưu khi không có điểm nào tối ưu hơn nó).

- **Ứng dụng:** Làm sao để chi phí sản xuất ô tô là thấp nhất nhưng chất lượng phải tốt nhất. Hoặc trong TSP, chi phí và thời gian di chuyển đều phải thấp nhất.

---

### CÁC PHƯƠNG PHÁP GIẢI BÀI TOÁN TỐI ƯU

Để giải bài toán tối ưu, ta có hai hướng chính: **giải một cách chính xác** và **giải một cách gần đúng chấp nhận được**. Hướng giải gần đúng được sử dụng vì bài toán tối ưu là bài toán khó, giải chính xác tốn rất nhiều chi phí và thời gian.

| Hướng giải                         | Phương pháp điển hình             | Đặc điểm                                                                                                             |
| :--------------------------------- | :-------------------------------- | :------------------------------------------------------------------------------------------------------------------- |
| **Giải chính xác**                 | Thuật toán **vét cạn**            | Duyệt toàn bộ không gian lời giải.                                                                                   |
|                                    | Thuật toán **nhánh cận**          | Phương pháp duyệt có chiến lược, có thời gian tốt hơn so với vét cạn.                                                |
| **Giải gần chính xác** (Heuristic) | Thuật toán **tham lam**           | Lựa chọn phương án tối ưu nhất ở trạng thái hiện tại, có thể rơi vào bẫy cục bộ.                                     |
|                                    | Thuật toán **mô phỏng luyện kim** | Lựa chọn dựa trên xác suất; các phương án tồi có thể được chọn với xác suất giảm dần, nhằm tránh rơi vào bẫy cục bộ. |

---

## GIỚI THIỆU VỀ TÍNH TOÁN TIẾN HÓA

**Tính toán tiến hóa (Evolutionary Computing)**
Đây là một kỹ thuật tìm kiếm lấy cảm hứng từ quá trình tiến hóa trong sinh học.
Hai toán tử cần lưu ý trong Evolutionary Computing là **toán tử lai ghép** (crossover operator) và **toán tử đột biến** (mutation operator).

**Thuật toán tiến hóa (Evolution Algorithm – EA)**

- EA là một thuật toán thuộc loại **heuristic** – là một thuật toán gần đúng.
- EA là giải pháp để tiết kiệm chi phí và có thể giải được bài toán một cách gần đúng, bởi vì tìm kiếm truyền thống có thể rất tốn kém thời gian và chi phí, và có thể sẽ không thể duyệt được toàn bộ không gian lời giải.
- Ví dụ: Google Map sử dụng thuật toán A\* (một loại thuật toán gần đúng) để tìm đường đi, đường đi gợi ý có thể chưa chắc là ngắn nhất nhưng khoảng cách không quá xa.

**Ý tưởng của EA:**
Từ thế hệ cha mẹ, EA tạo ra thế hệ con thông qua các phép toán **lai tạo** và sử dụng phép toán **đột biến** để đa dạng quần thể, rồi tiến hành chọn lọc cá thể để sinh ra các thế hệ kế tiếp.

**Đoạn mã giả của Thuật toán Tiến hóa (EA):**
begin Khởi tạo quần thể gồm các cá thể; Đánh giá các cá thể; while chưa đạt điều kiện dừng do Chọn ra các cá thể cha mẹ để sinh thế hệ kế tiếp; Tạo ra các cá thể con bằng các toán tử biến đổi; Đánh giá các cá thể con; Chọn ra các cá thể trong quần thể mới; end; end
**Giải thích các bước:**

- **Khởi tạo quần thể:** Xác định các thể tiềm năng trong không gian lời giải.
- **Chọn ra các cá thể cha mẹ:** Chọn ra hai hay nhiều (tùy thuật toán) cá thể tiềm năng trong quần thể.
- **Tạo ra các cá thể con:** Thực hiện phép toán lai tạo (crossover operator) trên cá thể cha mẹ, và toán tử đột biến (mution operator) trên chính cá thể con sinh ra.
- **Đánh giá các cá thể con:** Xác định độ thích nghi.
- **Chọn lại quần thể mới:** Việc lựa chọn sẽ dựa vào giá trị thích nghi của từng cá thể.
- **Điều kiện dừng:** Có thể là một giá trị tối ưu nào đó, hoặc thời gian thực thi tối đa, số lần đánh giá hàm thích nghi, hay độ đa dạng của quần thể giảm tới một ngưỡng nào đó.

**Mô hình hóa bài toán:**
Để giải quyết bài toán tiến hóa, ta phải mô hình hóa bài toán.

- Với bài toán TSP: Cá thể có thể mô hình hóa là **dãy hoán vị** của các thành phố.
- Với bài toán cái túi (KP): Cá thể có thể mô hình hóa là một **dãy bit**.

**Kiểu gen và Kiểu hình:**

- **Kiểu gen (Genotype):** Lời giải đã được mã hóa (ví dụ: số 4 được mã hóa bit là 0100, thì 0100 là kiểu gen). Thuật toán tiến hóa sẽ tìm lời giải tối ưu trên kiểu gen.
- **Kiểu hình (Phenotype):** Lời giải suy luận ra từ lời giải mã hóa (ví dụ: 4). Cần phải mã hóa ngược để ra được lời giải tường minh.

**Phân nhánh Thuật toán Tiến hóa (Evolutionary Algorithm):**
Tùy theo cách mô hình hóa bài toán, EA được phân thành nhiều nhánh khác nhau:

- **Thuật toán di truyền** (Genetic Algorithm – GA): Cá thể là chuỗi ký tự.
- **Chiến lược tiến hóa** (Evolution Strategy – ES): Cá thể là vector số thực.
- **Lập trình tiến hóa** (Evolutionary Programing – EP).
- **Lập trình di truyền** (Genetic Programing – GP).
- **Tiến hóa sai phân** (Differental Evolution – DE).
- **Tiến hóa đa nhiệm** (Evolution Multi – tasking – EM).
- ...

> Notebook này sẽ đi sâu hơn về các thuật toán GA, DE, NSGA-II
