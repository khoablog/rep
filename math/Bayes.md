**Luật Bayes** là cơ sở của **hệ thống loại trừ** và **mạng Bayes** mà mình sẽ đề cập đên đến sau này.

Nhắc lại **xác suất có điều kiện:**
$$
P(A|B)=\frac{P(AB)}{P(B)}
$$
**Định luật tổng xác suất:** Coi $A_1,A_2,..,A_k$ là các phần nhỏ của không gian mẫu $\Omega$, $\Omega=\cup_{j=1}^kA_j, \varnothing=\cap_{j=1}^kA_j$ và biến cố $B$ bất kỳ ta có:
$$
P(B)=\sum_{i=1}^kP(B|A_i)P(A_i)
$$

* *Chứng minh:* Định nghĩa $C_j=BA_j$, ta có $C_1,C_2,...,C_k$ không giao nhau và $B=\cup_{j=1}^{k}C_j$, do đó:
  $$
  P(B)=\sum_jP(C_j)=\sum_jP(BA_j)=\sum_jP(B|A_i)P(A_i)
  $$

**Luật Bayes:** Với $A_1,A_2,..,A_k,\Omega,B$ được định nghĩa ở trên, ta có:
$$
P(A_i|B)=\frac{P(B|A_i)P(A_i)}{\sum_jP(B|A_j)P(A_j)}
$$

* *Chứng minh:*
  $$
  P(A_i|B)=\frac{P(BA_i)}{P(B)}=\frac{P(B|A_i)P(A_i)}{P(B)}=\frac{P(B|A_i)P(A_i)}{\sum_jP(B|A_j)P(A_j)}
  $$



**Ví dụ:** Định nghĩa một email gồm *A1="spam"*, *A2="notspam"*. Theo kinh nghiệm, ta có $P(A_1)=0.3$ và $P(A_2)=0.7$. Gọi $B$ là biến cố có từ *"làm giàu"* trong một email. Theo kinh nghiệm: $P(B|A_1)=0.9$ và $P(B|A_2)=0.05$. Vậy nếu bạn nhận được một email có từ *"làm giàu"* thì xác suất nó là *"spam"* là bao nhiêu?

* *Lời giải:*

$$
P(A_1|B)=\frac{P(B|A_1)P(A_1)}{P(B|A_1)P(A_1)+P(B|A_2)P(A_2)}\approx0.89
$$

