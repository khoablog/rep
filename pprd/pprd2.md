



#### **2. Kiến trúc mạng và training**

<p>
    <img src='https://raw.githubusercontent.com/sunshineatnoon/Paper-Collection/master/images/fast-rcnn-arch.png' style='zoom:80%,'>
</p>

* Input của mạng **Fast R-CNN** là toàn bộ ảnh và tập hợp các *bounding-box* của vật thể.
* Đầu tiên mạng sẽ trích xuất *feature map* bằng *convolution* và *max-pooling*.
* *Feature map* của *bounding-box* sau đó được đưa qua lớp *RoI pooling*.
* Từng *feature vector* được nối với một chuỗi các *fully connected (fc) layers* rồi cuối cùng đưa ra 2 output:
  * Một output dùng *softmax* để đưa ra dự đoán *class* trong $(K+1)$ class ($K$ *classes* + *background*).
  * Output kia trả về 4 giá trị của từng $K$ object để xác định *bounding-box* của vật thể.

##### **2.1. RoI pooling layers**

* *RoI pooling* sử dụng *max-pooling* để convert *feature map* của *bounding-box* về kích cỡ $H\times K$ (thường được chọn $7\times7$).
* *RoI* được xác định là cửa sổ hình chữ nhật trong *feature map*. Đại diện cho nó là 4 tham số $(r,c,h,w)$, với $(r,c)$ là vị trí góc trên bên trái của hình chữ nhật, $(h,w)$ lần lượt là chiều cao và chiều rộng.
* *RoI max-pooling* chia *bounding-box* thành các cửa sổ nhỏ $h/H\times w/W$ rồi dùng *max-pooling* đưa output về dạng $H\times W$.
* Bạn đọc tìm hiểu rõ hơn tại <a href='https://deepsense.ai/region-of-interest-pooling-explained/'>đây</a>.

##### **2.2. Initializing from pre-trained**

* Các mạng *VGG* thường được dùng trong **image classification**. Khi thực hiện *pre-train* cho mạng **Fast R-CNN**, ta cần chỉnh sửa một chút như sau:
  * Lớp *max-pooling* cuối cùng được thay bằng *RoI pooling*.
  * Lớp *fc* và *softmax* cuối cùng được thay thế bằng 2 lớp như đã đề cập ở trên (*fc+softmax (K+1) classes* và *bounding-box*).
  *  Dữ liệu đầu vào phải bao gồm ảnh và list các *RoI* của từng ảnh.

##### **2.3. Fine-tuning for detection**

* ***Multi-task loss:***

  * Quy ước:
    * $u$ là class nhãn.
    * $v^k=(v^k_x,v^k_y,v^k_w,v^k_h)$  là *bounding-box* nhãn của class $k$ với $k\in[0,K]$.

  * Mạng **Fast R-CNN** trả về hai output:
    * $p=(p_0,...,p_K)$ là dự đoán phân phối xác suất của $K+1$ classes.
    * $l^k=(l^k_x,l^k_y,l^k_w,l^k_h)$ là *bounding-box* dự đoán của class $k$ với $k\in[0,K]$.

  * $t^u=(t^u_x,t^u_y,t^u_w,t^u_h)$ của class $u$ được định nghĩa:

    * $t^u_x=(v^u_x-l^u_x)/l^u_w$
    * $t^u_y=(v^u_y-l^u_y)/l^u_h$
    * $t^u_w=log(v^u_w/l^u_w)$
    * $t^u_h=log(v^u_h/l^u_h)$

  * Từng *RoI* được gán nhãn $u$ và *bounding-box* $v$. Hàm *loss* của *RoI* này sau khi được predict là:
    $$
    L(p,u,t^u,v)=L_{cls}(p,u)+\lambda[u\ge 1]L_{loc}(t^u,v)
    $$
    

    * $L_{cls}(p,u)=-log p_u$

    * $L_{loc}(t^u,v)$ phụ thuộc vào nhãn $v$ và predict $t^u$. $[u\ge 1]=1$ nếu $u\ge 1$ và bằng $0$ nếu ngược lại, nghĩa là: nếu $u=0$ (tức vật thể trong *RoI* là *background*) thì $L_{loc}$ được bỏ qua.

    * $L_{loc}(t^u,v)=\sum_{i\in \{x,y,w,h\}}smooth_{L_1}(t^u_i-v_i)$, với:

      
      $$
      \begin{align}
              smooth_{L_1}(x)=\begin{cases}
                  0.5x^2 &\text{if |$x$|<1}\\ 
                  |x|-0.5 & \text{otherwise,}
              \end{cases}
          \end{align}
      $$

    * $\lambda$ là tham số dùng để cân bằng giữa hai *loss*. Bounding-box $v$ thường được chuẩn hóa về *zero mean and unit variance*.

* ***Mini-batch sampling:***

  * Mỗi vòng lặp sẽ chọn ngẫu nhiên $2$ images trong bộ dataset. Dùng *selective search* [3] lấy mẫu 64 *RoIs* mỗi image.
    * Lấy $25\%$ những *RoIs* có *IoU* >0.5, gán nhãn class cho *RoI* đó  ($u\ge 1$).
    * Phần còn lại (IoU$\in[0.1,0.5)$) lấy *RoI* có *IoU* max, nhãn là $u=0$.
  * Trong suốt quá trình *training*, image *flipped* với xác suất 0.5. Không sử dụng *data augmention*.

* ***Backpropagation through RoI pooling layers:***

  * $x_i$ là in input thứ $i$ của lớp *RoI pooling*, $y_{rj}$ là lớp output thứ $j$ của *RoI* thứ $r$.

    <p>
        <img src='https://miro.medium.com/max/758/1*P99SvHcojXFv2LtgyiDQYQ.jpeg' style='zoom:100%,'>
    </p>

  * Do đó ta có: $y_{rj}=x_{i^*(r,j)}$ với $i^*(r,j)=argmax_{i'\in R(r,j)}$

  * Công thức đạo hàm sẽ là:
    $$
    \frac{\partial L}{\partial x_i}=\sum_r\sum_j[i=i^*(r,j)]\frac{\partial L}{\partial y_{rj}}
    $$

* ***SGD hyper-parameters:***

  * Mạng **Fast R-CNN** dùng *softmax* cho phân lớp và *bounding-box* được khởi tạo bằng *zero-mean Gaussian* với độ lệch chuẩn khoảng $[0.01,0.001]$, bias ban đầu bằng $0$.
  * Mỗi layer sử dụng *learning rate* là $1$ cho *weights* và $2$ cho *biases*. *Global learning rate* được chọn là $0.001$. Đi với đó là *momentum* $0.9$ và *parameter decay* $0.0005$ cho cả *weights* và *biases*.



#### Tham khảo

[1] R. Girshick, J. Donahue, T. Darrell, and J. Malik. Rich feature hierarchies for accurate object detection and semantic segmentation. In CVPR, 2014.

[2] <a href='https://towardsdatascience.com/fast-r-cnn-for-object-detection-a-technical-summary-a0ff94faa022'>Fast R-CNN for object detection | Toward Data Science</a>

[3] J. Uijlings, K. van de Sande, T. Gevers, and A. Smeulders. Selective search for object recognition. IJCV, 2013.

