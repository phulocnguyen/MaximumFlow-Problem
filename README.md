# Luồng cực đại - Maximum flow

1. [Khái niệm]
2. [Bài toán]
3. [Ứng dụng]
4. [Các tính chất của bài toán]
    - [Residual network - Mạng thặng dư]
    - [Augmenting path - Đường tăng luồng]
    - [Cut and min cut - Lát cắt và lắt cắt hẹp nhất]
    - [Relationship between min cut and max flow]
5. [Ford - Fulkerson Algorithm]
6. [Đánh giá]
7. [Cải tiến]
8. [**Bài toán liên quan**]
9. [Bài tập]

## Khái niệm

Ta gọi mạng là một đồ thị có hướng $G = (V, E)$, trong đó có duy nhất một đỉnh $s$ không có cung đi vào gọi là điểm phát, duy nhất một đỉnh $t$ không có cung đi ra gọi là đỉnh thu.

- **Khả năng thông qua (capacity)**
    
    Mỗi cung  $e = (u, v) \in E$  được gán với một số không âm $c(e) = c[u, v]$ gọi là khả năng thông qua của cung đó. Để thuận tiện cho việc trình bày, ta qui ước rằng nếu không có cung 
    
    $(u, v)$ thì khả năng thông qua $c[u, v]$ của nó được gán bằng 0
    
- **Luồng (flow)**
    
    Nếu có mạng $G = (V, E)$. Ta gọi luồng $f$ trong mạng $G$ là một phép gán cho mỗi cung 
    
    $e = (u, v) ∈ E$ một số thực không âm $f_{e} = f[u, v]$ gọi là luồng trên cung e, thoả mãn các điều kiện sau:
    
    - Luồng trên mỗi cung không vượt quá khả năng thông qua của nó: $\newline0 ≤ f[u, v] ≤ c[u, v] (∀ (u, v) ∈ E)$
    - Với mọi đỉnh $v$ không trùng với đỉnh phát $s$ và đỉnh thu $t$, tổng luồng trên các cung đi vào $v$ bằng tổng luồng trên các cung đi ra khỏi $v$:
    
    ![image1](D:\Workspace\MaximumFlow-Problem\images\image1.png)
    
- **Giá trị của luồng (value of flow)** là tổng luồng trên các cung đi ra khỏi đỉnh phát = tổng luồng trên các cung đi vào đỉnh thu.

![Untitled](D:\Workspace\MaximumFlow-Problem\images\Untitled.png)

## **Bài toán**

      Cho một mạng network $G = (V, E)$ là một đồ thị có hướng. Hãy tìm luồng $f*$ trong mạng với giá trị luồng lớn nhất. Luồng như vậy gọi là luồng cực đại trong mạng và bài toán này gọi là bài toán tìm luồng cực đại trên mạng

## **Ứng dụng**

- Quản lý lưu lượng mạng: Maximum flow được sử dụng để tối ưu hóa việc phân phối thông tin hay lưu lượng trong các mạng máy tính hoặc mạng điện thoại di động.
- Giao thông vận tải: Maximum flow có thể giúp tối ưu hóa việc chuyển hàng hoá hoặc người qua các tuyến đường vận tải.
- Lập lịch sản xuất: Maximum flow cũng có thể được sử dụng để tối ưu hóa quy trình sản xuất, giúp tiết kiệm chi phí và thời gian sản xuất.
- Thiết kế mạch điện: Maximum flow có thể giải quyết các vấn đề về thiết kế mạch điện, giúp tối ưu hóa việc truyền tín hiệu điện.
- Bảo vệ môi trường: Maximum flow cũng có thể được sử dụng để tối ưu hóa việc phân bổ tài nguyên và giảm thiểu ô nhiễm môi trường.

## **Các tính chất của bài toán**

> **Một số khái niệm quan trọng cần nắm**
> 

### **Lát cắt, lát cắt hẹp nhất**

- Ta gọi lát cắt $(X, Y)$ là một cách phân hoạch tập đỉnh $V$ của mạng thành hai tập rời nhau $X$ và $Y$, trong đó $X$ chứa đỉnh phát và $Y$ chứa đỉnh thu. Khả năng thông qua của lát cắt $(X, Y)$ là tổng tất cả các khả năng thông qua của các cung $(u, v)$ có $u ∈  X \enspace và \enspace v ∈ Y.$
- Lát cắt với khả năng thông qua nhỏ nhất gọi là lát cắt hẹp nhất

![image3](D:\Workspace\MaximumFlow-Problem\images\image3.png)

- Bài toán Lát cắt hẹp nhất $(Minimum cut)$ đi tìm lát cắt có khả năng thông qua nhỏ nhất

### **Mạng thặng dư (residual network)**

Giả sử $f$ là một luồng trong mạng $G = ( V, E )$. Từ mạng $G = ( V, E )$ ta xây dựng đồ thị có trọng số $G_{f}$ = $(V, E_{f})$ như sau

Xét những cạnh  $e = (u, v) \in E \enspace (c[u,v]>0)$

- Nếu $f[u,v] < c[u,v]$ thì ta thêm cung $(u, v)$ vào $E_{f}$ **với trọng số $c[u, v] - f[u, v]$, cung đó gọi là cung thuận. Về ý nghĩa, trọng số cung này cho biết còn có thể tăng luồng $f$ trên cung
    
    $(u, v)$ một lượng không quá trọng số đó.
    
- Xét tiếp nếu như $f[u, v] > 0$ thì ta thêm cung $(v, u)$ vào $E_{f}$ với trọng số $f[u, v]$, cung đó gọi là cung nghịch. Về ý nghĩa, trọng số cung này cho biết còn có thể giảm luồng $f$ trên cung $(u, v)$ một lượng không quá trọng số đó.
- **Cài đặt**

$$
\begin{align*} &ResidualNetwork(G(E,V),\enspace s,\enspace t, \enspace f(.)): \newline &~~~~ for \enspace each \enspace u\rightarrow v \in E \newline & ~~~~~~~~C_{f}[u,v] \leftarrow c[u,v] - f[u,v] \newline & ~~~~~~~~if \enspace f[u,v] > 0: \newline &~~~~~~~~~~~~ C_{f}[u,v] \leftarrow f[u,v] \end{align*}
$$

![image4](D:\Workspace\MaximumFlow-Problem\images\image4.png)

![image5](D:\Workspace\MaximumFlow-Problem\images\image5.png)

Đồ thị $G_{f}$ được gọi là mạng thặng dư

![image6](D:\Workspace\MaximumFlow-Problem\images\image6.png)

![image7](D:\Workspace\MaximumFlow-Problem\images\image7.png)

### **Đường tăng luồng (augmenting path)**

- Đường tăng luồng là một đường đi đơn từ đỉnh phát $s$ (source) đến đỉnh thu $t$ (sink) trong mạng thặng dư $G′$ mà kênh trên đường đi chưa bị bão hòa ( $f′[u,v] < c′[u,v]$, một kênh
    
    $e′(u,v)$ được gọi là bão hòa nếu $f\prime(u, v)=c\prime(u, v)$ ).
    
    Giả sử $P$ là một đường đi cơ bản từ đỉnh phát $s$ tới đỉnh thu $t$. Gọi $\Delta$ là giá trị nhỏ nhất của các trọng số của các cung trên đường đi $P$. Ta sẽ tăng giá trị của luồng $f$ bằng cách đặt:
    
    - $f[u, v] := f[u, v] + ∆$, nếu $(u, v)$ là cung trong đường $P$ và là cung thuận
    - $f[v, u] := f[v, u] - ∆$, nếu $(u, v)$ là cung trong đường  $P$ và là cung nghịch
    - Còn luồng trên những cung khác giữ nguyên
    
    Có thể kiểm tra luồng $f$ mới xây dựng vẫn là luồng trong mạng và giá trị của luồng $f$ mới được tăng thêm $\Delta$ so với giá trị luồng $f$ cũ. Ta gọi thao tác biến đổi luồng như vậy là tăng luồng dọc đường $P$, đường đi cơ bản $P$ từ $s$ tới $t$ được gọi là đường tăng luồng.
    
    - **Cài đặt**
    
$$
\begin{align*} 
& \text{Augment}(G(V,E), s, t, f(.), P, \Delta): \newline 
&~~~~ \text{Let } (s = u_{1} \rightarrow v_{1}), (u_{2} \rightarrow v_{2}), ..., (u_{k} \rightarrow v_{k} = t) \enspace \text{be arcs in } P \newline 
&~~~~ \text{for } i \leftarrow 1 \enspace \text{to } k: \newline 
&~~~~~~~~~ \text{if } (u_{i}, v_{i}) \in E: \newline 
&~~~~~~~~~~~~~~ f[u_{i}, v_{i}] \leftarrow f[u_{i}, v_{i}] - \Delta \newline 
&~~~~~~~~ \text{else} \newline 
&~~~~~~~~~~~~~ f[v_{i}, u_{i}] \leftarrow f[v_{i}, u_{i}] + \Delta 
\end{align*}
$$

    
    - **Ví dụ**: Với đồ thị tăng luồng  $G_{f}$ như trên, giả sử chọn đường đi $(1, 3, 4, 2, 5, 6)$. Giá trị nhỏ nhất của trọng số trên các cung là 2, vậy thì ta sẽ tăng các giá trị $f[1, 3]$,  $f[3, 4]$,
        
         $f[2, 5], f[5, 6]$ lên 2, (do các cung đó là cung thuận) và giảm giá trị $f[2, 4]$ đi 2 (do cung $(4, 2)$ là cung nghịch). Được luồng mới có giá trị 9.
        

![image6](D:\Workspace\MaximumFlow-Problem\images\image6.png)

![image8](D:\Workspace\MaximumFlow-Problem\images\image8.png)

### **Liên hệ giữa min cut problem và max flow problem**

- Xét lát cắt bất kì của đồ thị có đỉnh phát $s$ (source) thuộc tập $X$, đỉnh thu $t$ (sink) thuộc tập $Y$ và có một số cạnh nối giữa hai tập.
- Độ lớn của lát cắt là tổng khả năng thông qua của các cạnh nối hai tập đi từ $X -> Y$.

$$
c(X,Y) = \sum_{u\in X} \sum_{v\in Y} c[u,v]
$$

- Với mọi luồng hợp lệ $f$  và mọi lát cắt $(X,Y)$ ta có:

$$
\mid f\mid \enspace \leq \enspace c(X,Y)
$$

![image9](D:\Workspace\MaximumFlow-Problem\images\image9.png)

### Mincut - maxflow theorem:

> Với mọi mạng và 2 điểm đầu cuối $s, t$ giá trị của luồng $(s,t)$  cực đại bằng khả năng thông qua của lát cắt cực tiểu
> 

## **Ford-Fulkerson algorithm**

- Ý tưởng:
    
    > Hình dung thuật toán: Khởi tạo một luồng bất kỳ, sau đó cứ tăng luồng dọc theo đường tăng luồng, cho tới khi không tìm được đường tăng luồng nữa
    > 
    
    Vậy các bước của thuật toán tìm luồng cực đại trên mạng có thể mô tả như sau:
    
    - Bước 1: Khởi tạo:
        
        Một luồng bất kỳ trên mạng, chẳng hạn như luồng 0 (luồng trên các cung đều bằng 0), sau đó:
        
    - Bước 2: Lặp hai bước sau:
        
        Tìm đường tăng luồng $P$ đối với luồng hiện có ≡ Tìm đường đi cơ bản từ $s$ (source) tới $\newline t$ (sink) trên đồ thị tăng luồng, nếu không tìm được đường tăng luồng thì bước lặp kết thúc.
        
        Tăng luồng dọc theo đường $P$
        
    - Bước 3: Thông báo giá trị luồng cực đại tìm được
- Mã giả
    
$$
\begin{align*} 
& \text{FordFulkerson}(G(V,E), s, t): \newline 
&~~~~~ \text{for each} (u,v) \in E \newline 
&~~~~~~~~~ f[u,v] \leftarrow 0 \newline 
&~~~~~ \text{while true} \newline 
&~~~~~~~~~~ G_{f} \leftarrow \text{ResidualNetwork}(G, s, t, f) \newline 
&~~~~~~~~~~ P \leftarrow \text{FindAugmentingPath}(G_{f}, s) \newline 
&~~~~~~~~~~ \text{if} \enspace P = \emptyset \newline 
&~~~~~~~~~~~~~~~~ \text{return} \enspace f \newline 
&~~~~~~~~~~ \Delta \leftarrow +\infty \newline 
&~~~~~~~~~ \text{for each} (u, v) \in P \newline 
&~~~~~~~~~~~~~~~~~ \Delta \leftarrow \min(\Delta, C_{f}[u,v]) \newline 
&~~~~~~~~~ \text{Augment}(G, s, t, f, P, \Delta) 
\end{align*}
$$

    
- Cài đặt
    
    ```cpp
    void DFS(int u, sink) {
        // đánh dấu u đã được thăm
        visited[u] = true;
    
        // duyệt hết các đỉnh v có thể đến được từ u hay thỏa mãn điều kiện c[u][v] - f[u][v] > 0
        for( v in VertecesCanComeFromU ):
            if (!visited[v])
                trace[v] = u;
    }
    
    bool FindAugmentingPath(int source, int sink) {
        // khởi tạo mảng đánh dấu visited (false nếu chưa thăm, true nếu đã thăm)
        memset(visited, false, sizeof(visited));
        // Dùng DFS tìm đường tăng luông
        DFS(source, sink);
        return visited[sink];
        // trả về true nếu có một đường tăng luồng
        // trả về false nếu không có đường tăng luồng nào
    }
    
    void increase_flow(int mincap, int source, int sink) {
        //khởi tạo giá trị mincap vô cùng lớn
        mincap = inf;
        //Lấy khả năng thông qua nhỏ nhất để tăng luồng
        int u = sink;
        while (u != source) {
            int previousVertex = trace[u];
            mincap = min(mincap, c[previousVertex][u] - f[previousVertex][u]);
            u = previousVertex;
        }
    
        // tăng luồng
        while(sink != source) {
            int previousVertex = trace[sink];
            f[previousVertex][sink] += mincap;
            f[sink][previousVertex] -= mincap;
            sink = previousVertex;
        }
    
        // Nếu vần tồn tại đường tăng luồng
        while (FindAugmentingPath(s, t)) {
            increase_flow(s, t);
        }
    }
    ```
    

## **Đánh giá**

### **Tính đúng đắn**

- Ta có thể chứng minh 3 nhận định sau là tương đương:
    - $(i) \enspace f∗$ là luồng cực đại trên mạng
    - $(ii)$ Mạng thặng dư $G′$ không có đường tăng luồng
    - $(iii)$ Tồn tại lát cắt $s − t$ hẹp nhất trên $G′$
- **Chứng minh:**
    - $( i ) → ( ii ):$  vì nếu tồn tại đường tăng luồng thì ta có thể tăng luồng để được một luồng mới lớn hơn → trái với $(i)$
    - $( ii ) → ( iii )$: nếu giả sử không tồn tại lát cắt hẹp nhất ta có thể tìm được đường tăng luồng → $(ii)$ sai
    - $( iii ) → (i)$ : Ta có thể thấy $f(s,V′) = f(X,Y) ≤ c(X,Y)$, do đó $f(s,V′)$ là luồng cực đại vì nếu tồn tại một luồng $f∗ > f(s,V′)$ sẽ vô lý với nhận xét trong mục lát cắt $s−t$

### Tính bất biến

- Giá trị của luồng trên mỗi cạnh luôn là số nguyên
- Luồng trên mỗi cung không vượt quá khả năng thông qua của nó: $\newline0 ≤ f[u, v] ≤ c[u, v] (∀ (u, v) ∈ E)$
- Với mọi đỉnh $v$ không trùng với đỉnh phát $s$ và đỉnh thu $t$, tổng luồng trên các cung đi vào $v$ bằng tổng luồng trên các cung đi ra khỏi $v$:
    
    $\newline$$\sum_{u\in T^{-}(v)}f[u,v] = \sum_{w\in T^{+}(v)}f[v,w]$ 
    

$\newline$ Trong đó :    $T^{-}(v) = \{u\in V|(u,v)\in E\}$

$\enspace \enspace \enspace\enspace T^{+}(v) = \{w\in V|(v,w)\in E\}$

### Thời gian chạy (Time Complexity)

> Thực hiện lặp khi vẫn tìm được đường tăng luồng trong mạng.
> 

> Độ phức tạp thuật toán là $O(maxflow * E)$.
> 

### Worst case

> Trong trường hợp xấu nhất, chúng ta chỉ thêm được 1 đơn vị vào giá trị của luồng trong mỗi lần lặp. Vì vậy số đường tăng luồng có thể bằng với giá trị của luồng cực đại.
> 

## **Cải tiến**

### Thuật toán tìm đường tăng luồng FindAugmentingPath

- Sử dụng thuật toán tìm kiếm theo chiều sâu DFS
    
    Thuật toán tìm đường tăng luồng sử dụng tìm kiếm theo chiều sâu DFS đã được cài đặt ở trên
    
- Sử dụng thuật toán tìm kiếm theo chiều rộng BFS
    
    ```cpp
    void bfs(int source, int sink) {
        // khởi tạo mảng đánh dấu visited ( false nếu chưa thăm, true nếu đã thăm)
        memset(visited, false, sizeof(visited));
        queue.push(source);
        // đánh dấu source
        visited[source] = true;
    
        while (!queue.empty()) {
            
            u = queue.pop();
    
            // duyệt hết các đỉnh v có thể đến được từ u hay thỏa mãn điều kiện c[u][v] - f[u][v] > 0
            for( v in VertecesCanComeFromU ) {
                if (!visited[v]) {
                    queue.push(v);
                    visited[v] = true;
                    trace[v] = u;
                }
            }
        }
        
    bool FindAugmentingPath(int source, int sink) {
        // Dùng thuật toán bfs tìm đường tằng luồng từ source đến sink
        bfs(source, sink);
        return visited[sink];
    }
    ```
    
- Sử dụng hàng đợi ưu tiên Priority Queue
    
    ```cpp
    void pfs(int source, int sink) {
        // khởi tạo mảng đánh dấu visited ( false nếu chưa thăm, true nếu đã thăm)
        memset(visited, false, sizeof(visited));
        memset(mincap, 0, sizeof(mincap));
    
        //đẩy source vào priority_queue pq với giá trị luồng cực đại là vô cùng lớn
        pq.push([source, inf]);
    
        while (!queue.empty()) {
            uAndMinCapacity = queue.pop();
            minC = uAndMinCapacity[1];
            u = uAndCapacity[0];
    
            visited[u] = true;
    
            // duyệt hết các đỉnh v có thể đến được từ u hay thỏa mãn điều kiện c[u][v] - f[u][v] > 0
            for( v in VertecesCanComeFromU ) {
                if (!visited[v] && min(minC, c[u][v]-f[u][v]) > mincap[v]) {
                    mincap[v] = c[u][v]-f[u][v];
                    queue.push([v, mincap[v]]);
                    trace[v] = u;
                }
            }
        }
    }
    
    bool FindAugmentingPath(int source, int sink){
        // Dùng thuật toán pfs tìm đường tằng luồng từ source đến sink
        pfs(source, sink);
        return visited[sink];
    }
    ```
    

## Bài toán liên quan

1. **Mạng với nhiều điểm phát và nhiều điểm thu**
    
    Cho một mạng gồm $n$  đỉnh với $p$  điểm phát $A_{1}, A_{2},..., A_{p}$  và $q$ điểm thu $B_{1}, B_{2},...,B_{q}$ . Mỗi cung của mạng được gán khả năng thông qua là số nguyên. Các đỉnh phát chỉ có cung đi ra và các đỉnh thu chỉ có cung đi vào. Một luồng trên mạng này là một phép gán cho mỗi cung một số thực gọi là luồng trên cung đó không vượt quá khả năng thông qua và thoả mãn với mỗi đỉnh không phải đỉnh phát hay đỉnh thu thì tổng luồng đi vào bằng tổng luồng đi ra. Giá trị luồng bằng tổng luồng đi ra từ các đỉnh phát = tổng luồng đi vào các đỉnh thu. Hãy tìm luồng cực đại trên mạng.
    
2. **Tập đại diện**
    
    Một lớp học có $n$ bạn nam, $n$ bạn nữ. Cho $m$ món quà lưu niệm, $(n ≤ m)$. Mỗi bạn có sở thích về một số món quà nào đó. Hãy tìm cách phân cho mỗi bạn nam tặng một món quà cho một bạn nữ thoả mãn:
    
    - Mỗi bạn nam chỉ tặng quà cho đúng một bạn nữ
    - Mỗi bạn nữ chỉ nhận quà của đúng một bạn nam
    - Bạn nam nào cũng đi tặng và bạn nữ nào cũng nhận được quà, món quà đó phải hợp sở thích của cả hai người
    - Món quá nào đã được một bạn nam chọn thì các bạn nam khác không được chọn nữa.
3. **Lát cắt hẹp nhất** 
    
    Cho một đồ thị liên thông gồm $n$  đỉnh và $m$  cạnh. Hãy tìm cách bỏ đi một số ít nhất các cạnh để làm cho đồ thị mất đi tính liên thông
    

## Bài tập

[Bài tập liên quan đến luồng cực đại](https://www.notion.so/724f4e476e3b452dbb3d65700dc0ac8e?pvs=21) 

Practice makes perfect