# Kiến thức tại phần Modular Arithmetic
## Thuật toán Euclid
**1. Giới thiệu**
- Trong toán học, __thuật toán Euclid__ là một phương pháp hiệu quả để tìm ước chung lớn nhất của hai số nguyên.

**2. Xây dựng**
- Thuận toán Euclid được xây dựng trên nguyên lí : Ước của hai số nguyên cũng chính là ước của một trong hai số và hiệu của hai số cho trước.
```math
d=UC(a,b)=UC(a,a-b)=UC(b,a-b)
```
- Tuy nhiên để rút ngắn việc phải trừ $a$ nhiều lần $b$ thì ta sẽ thay thể bằng cách tìm số dư của $a$ chia cho $b$.
```math
a\ -\ \lfloor \frac{a}{b} \rfloor\ .\ b\ =\ a\ (mod \ b)
```

- Từ đó sau các vòng thì:
```math
\begin{cases}
a = b \\
b = a \% b
\end{cases}
```
- Dưới đây là ví dụ khi tìm $GCD(260,36)$:
<center>
	
| a | b | a%b | 
| :---: | :---: | :---: |
| 260 | 36 | 8 |
| 36 | 8 | 4 |
| 8 | 4 | 0 |
| 4 | 0 | None  |

</center>

**3. Thuật toán sử dụng Python**
```python
def GCD(a:int,b:int)->int:
    while b!=0:
        a,b=b,a%b
    return a
```
---
## Thuật toán Euclid mở rộng
**1. Giới thiệu**
- __Thuật toán Euclid mở rộng__ được sử dụng để giải phương trình Đi-ô-phăng có dạng:
```math
ax+by=c
```
- Trong đó:
    - $a,b,c$ là các hệ số nguyên
    - $x,y$ là các ẩn có giá trị nguyên
    - Nếu $d$ là $GCD(a,b)$ thì $d$ là ước của $c$

**2. Xây dựng**
- Để giải phương trình Đi-ô-phăng : $ax+by=c$ thì ta cần giải được phương trình : $ax+by=d$, khi phương trình này có nghiệm $(x_0,y_0)$ thì $(\frac{c.x_0}{d},\frac{c.y_0}{d})$ chính là nghiệm phương trình $ax+by=c$.
- Không mất tính tổng quát giả sử $a>b>0$, gọi $(a,b)=(r_0,r_1)$, $r_0$ chia $r_1$ = $q_1$ dư $r_2$.
- Tương tự ta có:
```math
r_0=r_1.q_1+r_2\\
r_1=r_2.q_2+r_3\\
...\\
r_{m-1}=r_m.q_m+r_{m+1}\\
r_m=r_{m+1}.q_{m+1}
```
Theo thuật toán Euclid, $r_{m+1}$ chính là $d$
- Tính ngược lại để tìm các $(x_i,y_i)$ sao cho:
```math
a.x_i+b.y_i=r_i
```
- Với $i=0$ thì phương trình sẽ là:
```math
a.x_0+b.y_0=r_0=a
```
- Ta chọn $(x_0,y_0)=(1,0)$
- Tương tự với $i=1$ thì phương trình sẽ là:
```math
a.x_1+b.y_1=r_1=b
```
- Ta chọn $(x_1,y_1)=(0,1)$
- Ta có: 
```math
r_i=r_{i+1}.q_{i+1}+r_{i+2}\\
\Rightarrow a.x_i+b.y_i=(a.x_{i+1}+b.y_{i+1}).q_{i+1}+r_{i+2}\\
\Rightarrow a.(x_i-x_{i+1}.q_{i+1})+b.(y_i-y_{i+1}.q_{i+1})=r_{i+2}
```
- Từ đây ta đặt:
```math
x_{i+2}=x_i-x_{i+1}.q_{i+1}\\
y_{i+2}=y_i-y_{i+1}.q_{i+1}
```
- Sau đó sử dụng truy hồi để tìm được $(x,y)$.

**3. Thuật toán sử dụng Python**
```python
def extended_gcd(a:int,b:int)->int:
    x0, x1, y0, y1 = 1, 0, 0, 1
    while b!= 0:
        q, a, b = a // b, b, a % b
        x0, x1 = x1, x0 - q * x1
        y0, y1 = y1, y0 - q * y1
    return x0, y0
```
---
## Định lý Fermat nhỏ
**1. Giới thiệu:**
- Định lý Fermat nhỏ khẳng định nếu $p$ là một số nguyên tố thì với mọi số nguyên $a$ bất kỳ ta có:
```math
a^p \equiv a\ (mod\ p)
```
**2. Chứng minh**
- Nếu $(a,p) \ne 1$ với $p$ là số nguyên tố ta có:
```math
a \equiv 0\ (mod\ p)\\
\Rightarrow a^p \equiv 0\ (mod\ p)
```
- Nếu $(a,p)=1$ :
    - $(1,p),(2,p),(3,p),...,(p-1,p) = 1 \Rightarrow (a,p),(2a,p),(3a,p),...,((p-1)a,p)=1 $
    - Chứng minh: $\forall m,n | 1 \le m \le n \le p-1$ thì $ma \equiv na \ (mod\ p)$ là không thể xảy ra 
        - vì nếu $ma \equiv na \ (mod\ p)$
        - $\Rightarrow a(m-n)\equiv 0\ (mod\ p)$
        - $\Rightarrow m \equiv n\ (mod\ p)$
        - $\Rightarrow$ Vô lý 
    - Từ đó có :
```math
a^{p-1}.(p-1)!\equiv(p-1)!\ (mod \ p)\\
\Rightarrow a^{p-1} \equiv 1\ (mod \ p)
\Rightarrow a^p \equiv a\ (mod \ p)
```
---
## Nghịch đảo modulo
**1. Giới thiệu**
- Xét số nguyên dương $m$, xét các số nguyên trên module $m$ (từ 0 đến $m-1$), lấy một số nguyên $a$, ta gọi nghịch đảo module $m$ của $a$ là $a^{-1}$ là số nguyên : 
```math
a.a^{-1}\equiv 1\ (mod\ m)
``` 

**2. Tìm nghịch đảo modulo bằng Extended GCD**
- Nếu $gcd(a,m)=1$ ta luôn có đẳng thức:
```math
a.x+m.y=1
```
- Lấy $mod\ m$ cả 2 vế:
```math
a.x\equiv1(mod\ m)
```
$\Rightarrow$  $x$ là nghịch đảo module $m$ của $a$
- Code:
```python
def extended_gcd(a:int,b:int)->int:
    x0, x1, y0, y1 = 1, 0, 0, 1
    while b!= 0:
        q, a, b = a // b, b, a % b
        x0, x1 = x1, x0 - q * x1
        y0, y1 = y1, y0 - q * y1
    return x0, y0

def modular_inverse(a:int,m:int)->int:
    x,y = extended_gcd(a,m)
    return (x%m)
```

**3. Tìm nghịch đảo module sử dụng định lý Euler**
1. Định lý Euler
    - Nếu $gcd(a,m)=1$ thì ta có: $a^{\phi (m)} \equiv 1\ (mod\ m)$ trong đó $\phi$ là hàm phi Euler
    - Trong lí thuyết số, hàm Euler của một số nguyên $n$ là số các số nguyên tố cùng nhau với $n$
    - Nếu $m$ là số nguyên tố thì $\phi m = m-1$
2. Tìm nghịch đảo module
    - Vậy là ta có: $a^{m-1} \equiv 1\ (mod\ m) \Rightarrow a^{-1} \equiv a^{m-2}\ (mod\ m)$
    - Sử dụng thuật toán để tính được $a^{m-2}\  mod\ m $
    - Code tìm $a^{m-2}$
```python
def binary_de_quy(num: int, expo : int) -> int:
    if expo == 0:
        return 1
    res = binary_de_quy(num, expo // 2)
    if expo % 2 == 1:
        return res*res*num
    else:
        return res*res
```
---
## Thặng dư bậc hai
**1. Giới thiệu**
- Cho $a \in \mathbb{Z}$ ta nói $a$ là thặng dư bậc hai nếu tồn tại $x$ sao cho $x^2 \equiv a \ (mod\ p)$.
- Ngược lại nếu không tồn tại $x$ thì ta sẽ gọi $a$ là phi thặng dư bậc hai.

**2. Tính chất**
1. Với $p$ là số nguyên tố lẻ, ta đều có số lượng thặng dư bậc 2 và phi thặng dư bậc 2 là bằng nhau.
    - Chứng minh: Giả sử ta có $b^2=a$ mà $(-b)^2=a$ mà $b \ne -b$ , mỗi thặng dư bậc 2 có ít nhất 2 căn bậc 2 mà nếu số lượng thặng dư bậc 2 nhiều hơn một nửa module thì khi căn sẽ tạo ra nhiều hơn số module.

**3. Thuật toán**
```python
import math

def check_chinhphuong(n):
	check = False
	if (int(math.sqrt(n)))**2 == n:
		check = True
	return check

def quadratic_residue(n,p):
	limit = p**2
	check = False
	while n < limit:
		n = n+p
		if (check_chinhphuong(n)):
			print(math.sqrt(n)%p,p-(math.sqrt(n)%p))
			check = True
			break
	return check
```
---
## Biểu tượng Legendre
**1. Giới thiệu**
- Biểu tượng Legendre được định nghĩa bởi số nguyên tố $p$ và số nguyên $a$ theo
```math
(\frac{a}{p}) \equiv a^{\frac{p-1}{2}}\ (mod\ p)
```
**2. Tính chất:**
- $(\frac{a}{p})=1$ nếu $a$ là thặng dư bậc hai của $p$
- $(\frac{a}{p})=-1$ nếu $a$ là phi thặng dư bậc hai của $p$
- $(\frac{a}{p})=0$ nếu $a\equiv 0\ (mod\ p)$
- Với mọi số nguyên b: $(\frac{a}{p})(\frac{b}{p})=(\frac{ab}{p})$
- Với mọi số $r$ sao cho $(p,r)=1$ thì $(\frac{a}{p}) = (\frac{a.r^2}{p})$

**3. Thuật toán tìm xác định $x$ mà $x^2 \equiv a\ (mod\ p)$, với $p$ là số nguyên tố lẻ** 
1. $p \equiv 3\ (mod\ 4)$
    - Đặt $p=4k+3$ với $k$ là số nguyên
    - Thay vào ta có:
```math
a^{\frac{p-1}{2}}.a=a^{\frac{p+1}{2}}=a^{\frac{4k+4}{2}}=a^{2k+2}=(a^{(k+1)})^2 \equiv a
 (mod\ p)\\
 \Rightarrow x=a^{k+1}
```
2. $p \equiv 1\ (mod\ 4)$
    - Ở trường hợp này, ta cần sử dụng đến thuật toán [Tonelli-Shanks](https://en.wikipedia.org/wiki/Tonelli%E2%80%93Shanks_algorithm).
    - Ta phân tích $p-1$ thành $Q.2^n$
    - Ta tính $t = n^Q (mod\ p)$; $R = n^{(Q+1)/2} \Rightarrow R^2 = n^{Q+1}$
    - $\Rightarrow R^2 = n.t$ vậy nếu $t=1$ thì ta có thể tìm được $x=R$
    - Nếu $t \ne 1$ thì ta sẽ điều chỉnh $R,t$ bởi $z$ (phi thặng dư bậc 2 module $p$)
    - Ta tìm $g = z^Q (mod\ p)$ để điều chỉnh $R$ và $t$
    - Đặt $M=s$, ta lặp lại các bước cho đến khi $t=1$
    - Vòng lặp như sau:
        - Tính $t^{2^{M-1}}$ xem có đồng dư với 1 theo module $p$ hay không, nếu có ta có thể giảm $M -> M-1$ và lặp lại, nếu không, ta sẽ điều chỉnh $R$ và $t$.
        - Tìm số $i$ nhỏ nhất sao cho $t^{2^i} \equiv -1\ (mod\ p)$ , $i$ sẽ nằm trong $[0,M-1]$
        - Ta sử dụng $g$ với $g^{2^s} \equiv 1\ (mod \ p)$
        - Tính $b = g^{2^{s-i-1}}\ mod\ p$, khi đó $b^{2^{i+1}} \equiv g^{2^s} \equiv 1 \ (mod\ p)$ nên $b^{2^i} \equiv -1\ (mod \ p)$
        - Tiếp theo ta gán $R = R.b (mod \ p)$ và $t = t.b.b\ (mod\ p)$
        - Sau khi gán, ta có thể giảm $M$ xuống $i$ và lặp lại chu trình đó cho đến khi $t=1$ thì $x=R$

```Python
def check(a,p):
	if a%p==0:
		return True
	return pow(a,(p-1)//2,p)==1

def tonelli_shanks(a, p):
    if p % 4 == 3:
        k = (p - 3) // 4
        x = pow(a, k + 1, p)
        return x
    else:
        Q, S = p - 1, 0
        while Q % 2 == 0:
            S += 1
            Q = Q // 2

        z = 2
        while check(z, p):
            z += 1

        M = S
        c = pow(z, Q, p)
        t = pow(a, Q, p)
        R = pow(a, (Q + 1) // 2, p)

        while t != 1:
            i = 0
            temp = t
            while temp != 1:
                temp = pow(temp, 2, p)
                i += 1

            b = pow(c, pow(2, M - i - 1, p), p)
            c = pow(b, 2, p)
            t = (t * c) % p
            R = (R * b) % p
            M = i

        return R
```
---
## Chinese Remainder Theorem
**1. Giới thiệu**
- Giải quyết bài toán tìm một số nguyên thỏa mãn nhiều điều kiện đồng dư cùng lúc.
- Định lý: Cho $m_1,m_2,...,m_n$ các cặp đôi một nguyên tố cùng nhau $(m_i,m_j)=1$  và một hệ gồm $n$ phương trình
```math
x \equiv a_1\ (mod\ m_1)\\
x \equiv a_2\ (mod\ m_2)\\
...\\
x \equiv a_n\ (mod\ m_n)
```
có một nghiệm duy nhất module $M$ với $M = m_1.m_2....m_n$
- Lấy $b_i = \frac{M}{m_i}$ và $b_i^{'} \equiv b_i^{-1}\ (mod\ m_i)$ thì ta tìm được nghiệm $x$ duy nhất bằng công thức
```math
x = \sum_{i=1}^n a_i.b_i.b_i^{'}\ (mod\ M)
```
**2. Cách xây dựng**
- Ta xây dựng nghiệm $x$ theo tổng của các số hạng $t_i$ và $t_i$ đáp ứng các tiêu chí:
    - Đáp ứng phương trình chính nó : $t_i \equiv a_i(mod\ m_i)$
    - Là tích của các module $m_j$ với $j \ne i$ để tránh trường hợp trùng đồng dư với các $t_j$ khác
```math
t_i \equiv a_i\ (mod\ m_i)\\
t_i \equiv 0\ (mod \ m_j) \ \forall \ j \ne\ i
```
- Ta có: $t_i = a_i.b_i$ , tuy nhiên sau khi nhân với $b_i$ thì $t_i$ không còn đồng dư $a_i$ theo module $m_i$ nữa, cho nên ta cần tìm một số $k$ sao cho:
```math
a_i.b_i.k\equiv a_i\ (mod\ m_i)\\
\Rightarrow b_i.k \equiv 1\ (mod\ m_i)
```
- Mà ta có $(b_i,m_i)=1$ cho nên tìm $k$ là hoàn toàn có thể $(b_i^{'})$
Cho nên công thức tìm nghiệm chính là:
```math
x = \sum_{i=1}^n t_i \ (mod\ M)= \sum_{i=1}^n a_i.b_i.b_i^{'}\ (mod\ M)
```

**3. Chứng minh**
1. Chứng minh sự tồn tại
    - Có $b_i = \frac{M}{m_i}$ cho nên $m_j\ |\ b_i\ \  \forall i \ne j $
```math
x=\displaystyle \sum_{i=1}^{n}b_ib_i^{'}a_i\ (mod\ m_i)
```
mà $b_ib_i^{'} \equiv 1\ (mod\ m_i)$ 
```math
\Rightarrow x \equiv a_i\ (mod\ m_i)
```
2. Chứng minh duy nhất
- Giả sử tồn tại 2 nghiệm x, y sao cho $x\equiv y\equiv a_i\ (mod\ m_i)$ với mọi $i$
- Vì các số $m_i$ nguyên tố cùng nhau nên $x\equiv y\ (mod\ M)$
- Cho nên $x,y$ chỉ khác nhau về bội của $M$


**4. Thuật toán**
```Python
def extended_gcd(a:int,b:int)->int:
    x0, x1, y0, y1 = 1, 0, 0, 1
    while b!= 0:
        q, a, b = a // b, b, a % b
        x0, x1 = x1, x0 - q * x1
        y0, y1 = y1, y0 - q * y1
    return x0, y0

def modular_inverse(a:int,m:int)->int:
    x,y = extended_gcd(a,m)
    return (x%m)

def CRT(a: list, m:list)->int:
	M = 1
	for i in range(len(a)):
		M = M*m[i]

	p = [M//m[i] for i in range(len(m))]
	p1 = [modular_inverse(p[i],m[i]) for i in range(len(m))]
	result=0
	for i in range(len(a)):
		result = result + p[i]*p1[i]*a[i]
	return result%M 
```
