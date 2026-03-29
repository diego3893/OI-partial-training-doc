# 算法竞赛参考doc

## 0 目录

- STL && C++常用库函数 1
- 常有数据结构模板 5
- 常用算法模板 17

## 1 STL && C++常用库函数

- 小节名称为头文件
- 所有STL都可以手搓，详见第二章

### 1.0 algorithm

- 算法

```c++
#include<algorithm>
using namespace std;
	sort(begin, end, cmp); //快排
	swap(a, b); //交换
	max(a, b); //返回最大值
	min(a, b); //返回最小值
    reverse(begin, end); //反转
```

```C++
//其中sort用的非常多，默认升序
//当对数组arr排序时
int n = 10;
int arr[n];
sort(arr, arr+n); //升序
bool cmp(const int &a, const int &b){
    return a>b;
}
sort(arr, arr+n, cmp); //降序
//对容器排序（vector）
vector<int> v;
sort(v.begin(), v.end()); //升序
sort(v.begin(), v.end(), greater<int>()); //降序
//对结构体排序
struct Node{
	int x, y;
};
Node coord[10];
bool cmp(const Node& a, const Node& b){
	if(a.x == b.x){
        return a.y>b.y;
	}
    return a.x > b.x;
}
sort(coord, coord+10, cmp); //降序，x优先级大于y
```

### 1.1 cstring && string

- 字符串

```c++
#include<cstring>
using namespace std;
	int arr[10], target[10];
	memset(arr, 0, sizeof(arr)); //按字节初始化，只用于0或者-1的赋值
	memcpy(target, arr, sizeof(arr)); //将arr复制到
	
    char src[10]， dest[10];
	int length = strlen(src); //strlen只能用于求char[]的长度
	strcpy(dest, src); //复制src到dest
	strncpy(dest, src, 5); //最多从src复制5个字符到dest
	strcmp(s1, s2); //字典序比较s1,s2，大于返回>0，等于返回0，小于返回<0
```

```c++
#include<string>
#include<iostream>
#include<cstdio>
using namespace std;
	string str = "hello world"; //字符串类型，几乎等价于char[]，元素支持下标访问
	cout << str;
	printf("%s", str.c_str()); //注意：string和%s不互通，要调用c_str()
	int len = str.size(); //str的长度
	bool flag = str.empty(); //判断str是否是空串

	string s1, s2;
	s1==s2 //string类型可以用逻辑运算符直接比较，按照字典序
    
    str.substr(pos, len); //返回从pos开始的长度为len的子串，如果省略len取到末尾
	
	//遍历
	for(int i=0; i<s.size(); ++i){
        printf("%c", s[i]);
	}
	for(char c: s){
        printf("%c", c);
    }
	for(auto it = s.begin(); it != s.end(); it++){
        cout << *it;
    }
```

### 1.2 vector

- 动态数组

```c++
#include<vector>
using namespace std;
	vector<int> v1; //空
	vector<int> v2(3, 0); //{0, 0, 0}
	v[i] //下标访问
    v.front();v.back(); //返回第一个/最后一个元素
	int size = v.size();
	bool flag = v.empty();
	v1.push_back(val); //在末尾添加元素
	v1.pop_back(); //删除末尾元素

	//可以用来存图
	vector<int> g[1000];
	g[u].push_back(v); //存u->v的边
```

### 1.3 map

- 键值映射

```c++
#include<map>
#include<string>
#include<cstdio>
using namespace std;
	map<int, string> m = {{1, "hello"}}; //<key, val>
	printf("%s", m[1]); //output:hello
	m[2] = "world"; //m[key] = val用于修改或者插入元素 注意：m[k]的访问在k不存在时会插入一个{k, {}}对
	auto it = m.find(1); //查找m中键为1的元素
	printf("%d: %s", it->first, it->second); //output: 1: hello
```

### 1.4 stack

- 栈

```c++
#include<stack>
using namespace std;
	stack<int> s;
	s.push(val); //压入元素
	int res = s.top(); //栈顶元素
	bool flag = s.empty(); //是否空
	s.pop(); //弹出栈顶
	//不支持遍历，没有迭代器
```

### 1.5 queue

- 队列

```c++
#include<queue>
using namespace std;
	queue<int> q;
	q.push(val); //队尾入队
	q.pop(); //队头出队
	q.front(); q.back(); //访问首尾
```

### 1.6 priority_queue

- 优先队列，默认大根堆
- 头文件是`queue`

```c++
#include<queue>
using namespace std;
	priority_queue<int> max_heap; //大根堆
	priority_queue<int, vector<int>, greater<int>> min_heap; //小根堆
	push(),pop(),top(),size(),empty() //同其他STL
```

### 1.7 cmath

- 数学库

```c++
#include<cmath>
using namespace std;
	sqrt(x);
	pow(double x, double y); //幂函数
	floor(x); //向下取整
	ceil(x); //向上取整
```

### 1.8 C++新增语法

```c++
//申请内存
Node *p = new Node;
Node *p = (Node*)malloc(sizeof(Node)); //等价

cin && cout //#include<iostream>
```

- 结构体的构造函数和运算符重载

```c++
#include<algorithm>
struct Node{ //不需要typedef也可以直接用Node声明变量
    int x;
    int y;
    Node(int x_ = 0, int y_ = 0): x(x_), y(y_) {} //构造函数，可以理解为初始化
    int operator*(const Node& other) const{ //重载*为向量的内积
        return x*other.x+y*other.y;
	}
    bool operator>(const Node& other) const{
        if(x == other.x){
            return y > other.y;
        }
        return x > other.x;
    }
};

Node n1; //n1.x = n1.y = 0
Node n2(1, 2); //n2.x = 1, n2.y = 2
Node n[100];
sort(n, n+100, greater<Node>()); //按照x高优先级、降序排列
```

- `greater<>`
  - 作为模板参数时，`priority_queue<int, vector<int>, greater<int>> min_heap`，直接调用
  - 作为函数参数时，先实例化，即用`()`，比如`sort(n, n+100, greater<Node>())`

## 2 常用数据结构

- 基本都是静态结构，**不会**涉及链表（数据范围给了为什么要动态呢对吧），动态数据结构使用`vector`
- 平衡树红黑树还没学谁来教教我

### 2.0 栈

- STL为`stack`

```c++
int sta[100], top = 0, size = 100;
sta[top++] = val;
res = sta[top--];
if(!top){
    bool isEmpty = 1;
}
if(top == size){
    bool isFull = 1;
}
```

### 2.1 队列

- STL为`queue`

```c++
int que[100], head = 0, tail = 0, size = 100, cnt = 0; //仅使用head==tail无法区分空/满
que[(tail++)%size] = val; cnt++; //队尾入队，尾指针++，取模是模拟循环队列，避免浪费head之前的空间
res = que[(head++)%size]; cnt--; //获取队首并且出队
if(head==tail && !cnt){
    bool isEmpty = 1;
}
if(head==tail && cnt==size){
    bool isFull = 1;
}
```

### 2.2 单调队列

- STL为`deque`，需要主动维护单调性

- 内部单调的双端队列
- 用于处理滑动区间中的最值

```C++
// 以求长度为k的区间最大值为例，即队列内部单调递减
// 单调队列存储下标
int a[100];
int head = 0, tail = 0;
int q[100];
for(int i=0; i<n; ++i){
    while(head<tail && a[q[tail-1]]<=a[i]){
        tail--; // 维护单调性，将比当前元素小的出队
	}
    q[tail++] = i;
    if(q[head] <= i-k){
        head++; // 超出窗口，当前区间为[i-k+1, i]
    }
    if(i >= k){
        printf("%d", a[q[head]]); // 单调递减队列的队首是最大值
	}
}
```

### 2.3 前缀和 && 差分

- 前缀和用于快速求区间和，O(1)查询；区间修改为O(n)
- 差分用于区间修改，复杂度O(1)；差分还原就是求前缀和，O(n)

```c++
int arr[n];
int diff[n];
int pre[n];
diff[1] = a[1];
for(int i=2; i<=n; ++i){
    diff[i] = arr[i]-arr[i-1]; //差分
}
//如果[l,r]中每个元素都值都-val
diff[l] -= val;
diff[r+1] += val;
//对diff求前缀和可以还原出arr

pre[0] = 0;
for(int i=1; i<=n; ++i){
    pre[i] = pre[i-1]+arr[i]; //前缀和
}
int res = pre[r]-pre[l-1]; //区间[l,r]的元素之和
```

### 2.4 哈希表

- 散列表
- 通过哈希函数获得映射关系后存放数据（算法部分写）
- 表头对应不同的哈希值，表头后一般接一个链表来存哈希冲突的元素
- 感觉可能用不上就不细写了（偷懒）

### 2.5 堆

- STL为`priority_queue`
- 本质是一棵完全二叉树，任意节点都不小于其子节点（大根堆）
- 由于完全二叉树的性质，其维护最值的复杂度为O(logn)，用于不断修改并查询最值的情况
  - 维护大小为K的小根堆，可以快速求出N个数中前K大的数（Top-K）
- 为什么叫做**优先队列**？*最值优先出队*

```c++
//数组如何模拟树的结构？
heap[1] // 根
heap[i] // 节点i
heap[i<<1] // 2i为节点i的左儿子
heap[i<<1|1] // 2i+1为节点i的右儿子
heap[i>>1] // i/2为节点i的父亲
// 需要怎么维护？ 1.压入一个元素，从叶子节点向上调整； 2.弹出一个元素，从根节点开始向下调整
void up(int p){
    while(p>1 && heap[p]>heap[p>>1]){
        swap(heap[p], heap[p>>1]);
        p >>= 1; //p为插入元素的下标，不断寻找其父亲节点并比较以维护大根堆的性质，直到根节点
    }
}
void down(int p){
    int s = p<<1; //左儿子
    while(s <= size){
        if(s<size && heap[s+1]>heap[s]){ // sz是当前堆的大小，即堆内元素的个数
            s++; // 比较左右儿子，取较大的那个
        }
        if(heap[s] > heap[p]){
            swap(heap[s], heap[p]);
            p = s;
            s = p<<1;
        }else{
            break; // 满足性质，不用再调整
        }
    }
}

// 如何建堆？
int arr[100]; //假设arr现在乱序，我们就在arr中建立堆
for(int i=n/2; i>=1； --i){ // n/2为最后一个非叶子节点（叶子节点的down操作无意义）
    down(i);
}

//pop & push & get_max
void push(int x){
    heap[++size] = x; //从末尾（叶子）加入，向上调整
    up(size);
}
int get_max(){
    return heap[1];
}
void pop(){
    heap[1] = heap[size--]; // 删除原本的根，并顺手将size-1
    down(1); // 从根节点向下调整
}
```

### 2.6 树状数组

- 数组和前缀和优点的结合体
- O(logn)完成单点修改和区间查询
  - 数组单点修改O(1)，区间查询O(n)
  - 前缀和单点修改O(n)，区间查询O(1)
- 树状数组存原数组即求区间和，存差分即求原数组
- 动画讲解：[五分钟丝滑动画讲解 | 树状数组]( https://www.bilibili.com/video/BV1ce411u7qP/?share_source=copy_web&vd_source=8323cdc98c662fa047cc74f5e35db1b5)

```c++
int lowbit(int x){
    return x&(-x); // 获得x二进制表示中最低位1的位置（最低位为第1位）
}
//example: 6=0110, -6=1010，6&(-6)=0010=2
//对于add，在x到n之间被维护的元素，恰好是包含被单点修改元素的“部分前缀和”
void add(int x, int v){ // 单点修改，在位置x加上v
    for(; x<=n; x+=lowbit(x)){
        tree[x] += v;
	}
}
int query(int x){ // 查询[1,x]的和
    int res = 0;
    for(; x>0; x-=lowbit(x)){
        res += tree(x);
	}
    return res;
}
int range_query(int l, int r){ // 查询[l,r]和
    return query(r)-query(l-1);
}

//建立树状数组
//法1
for(int i=1; i<=n; ++i){
    add(i, a[i]); 
}
//法2
for(int i=1; i<=n; ++i){
    tree[i] += a[i];
    int j = i+lowbit(i);
    if(j <= n){
        tree[j] += tree[i];
    }
}
```

### 2.7 线段树

- 树状数组plus
  - 高效的区间修改和区间查询
- 一棵完全二叉树，对大小为n的数组，其线段树大小约4n
- 左右儿子的表示和堆完全一样（因为都是完全二叉树）
- 左右儿子与父亲区间的关系：父亲[l,r]，长度len=r-l+1，左儿子为[l, l+len/2]，右儿子为[l+len/2+1, r]
- 维护lazy_tag，让修改停留在当前被使用的区间，不必一次修改到底，减少无效开销

```c++
struct Node{
    int l, r;
    long long sum, lazy;
}tree[MAXN*4];

void pushup(int p){ //向上修改区间和：父亲的区间和就是其左右儿子的区间和之和
    tree[p].sum = tree[p<<1].sum + tree[p<<1|1].sum;
    return;
}
// 修改时注意修改值和lazy_tag
void pushdown(int p){
    if(tree[p].lazy){
        // 下传给左儿子
        tree[p<<1].sum += tree[p].lazy*(tree[p<<1].r-tree[p<<1].l+1);
        tree[p << 1].lazy += tree[p].lazy;
        // 下传给右儿子
        tree[p<<1|1].sum += tree[p].lazy*(tree[p<<1|1].r-tree[p<<1|1].l+1);
        tree[p<<1|1].lazy += tree[p].lazy;
        // 清空当前标记
        tree[p].lazy = 0;
    }
    return;
}
// 建立线段树
void build(int p, int l, int r){
    tree[p] = {l, r, 0, 0};
    if (l == r){
        tree[p].sum = a[l];
        return;
    }
    int mid = (l+r)>>1;
    build(p<<1, l, mid); // 递归处理子树
    build(p<<1|1, mid + 1, r); 
    pushup(p);
}

void update(int p, int l, int r, int k) {
    if (tree[p].l >= l && tree[p].r <= r) { // 当前区间被包含，则sum要加上k*区间长度
        tree[p].sum += (long long)k*(tree[p].r-tree[p].l+1);
        tree[p].lazy += k;
        return; // 子区间不被使用，不要修改
    }
    // 区间没有完全包含，则需要分别处理子区间
    pushdown(p); // 下传标记
    int mid = (tree[p].l+tree[p].r)>>1;
    if (l <= mid){ // 区间与左子树有交集才处理
        update(p<<1, l, r, k);
    }
    if (r > mid){ // 区间与右子树有交集才处理
        update(p<<1|1, l, r, k);
    }
    pushup(p); // 修正父节点
}

long long query(int p, int l, int r) {
    if (tree[p].l>=l && tree[p].r<=r){
        return tree[p].sum;
    }
    pushdown(p);
    int mid = (tree[p].l+tree[p].r)>>1;
    long long res = 0;
    if (l <= mid){
        res += query(p<<1, l, r);
    }
    if (r > mid){
        res += query(p<<1|1, l, r);
    }
    return res;
}

//建树
build(1, 1, n);
//查询[1,3]
query(1, 1, 3);
//修改
update(1, 1, 3, 5);
```

### 2.8 并查集

- 我说这是求等价类有没有懂的
- 同一棵树中的所有成员属于一个等价类

```c++
// fa(father)数组用于存储该节点的父亲
void init(){
    for(int i=1; i<=n; ++i){
        fa[i] = i;
    }
    return;
}
int find(int x){
    if(fa[x] == x){
        return x;
    }
    return fa[x] = find(fa[x]); // 递归查找并且路径压缩，因为同一棵树的节点最终的结果都是根节点
}
void merge(int x, int y){
    int rootX = find(x);
    int rootY = find(y);
    if(rootX != rootY){
        fa[rootX] = rootY; // 把X的树接在Y下面，即rootY变成二者共同的父亲，也就是合并
    }
    return;
}
```

### 2.9 图的存储

- 邻接矩阵
- 邻接表
  - 广义表
  - 数组模拟链表
- 边集数组

```c++
//邻接矩阵
int g[n][n];
for(int i=0; i<n; ++i){
    for(int j=0; j<n; ++j){
        g[i][j] = w; // g[i][j]表示i->j的边权
    }
}
```

```c++
// 邻接表
// STL 用vector，不赘述
// 数组模拟（类似静态链表）
// 初始化head为全-1
memset(head, -1, sizeof(head));

void add(int u, int v, int w){ // 存u->v权重为w的边
    ver[Index] = v; 
    edge[Index] = w;
    next[Index] = head[u];
    head[u] = Index++; // 相当于把链表节点插入到表首
}

//遍历从u出发的所有边
for(int i=head[u]; i!=-1; i=next[i]){
    int v = ver[i], w = edge[i];
}
```

```c++
// 边集数组，显然的
struct Edge{
    int u, v, w;
};
```

### 2.10 平衡树

- 一种二叉搜索树BST，保证左子树值小于根，右子树值大于根
- 通过平衡树的高度来保证O(logn)的复杂度，避免最坏情况下退化为O(n)
- 提供Treap和FHQ Treap的模板（gemini写的）
  - treap就是tree+heap，通过维护优先级的大根堆性质来平衡树的高度，优先级为一个随机数来模拟随机插入，避免树退化为链（为什么是随机数？想想快排的pivot）
  - 传统的treap通过左旋、右旋来维护堆的性质，对区间操作不太友好（旋转操作的图可以参考《数据结构》教材）
  - FHQ treap通过分裂和合并来维护，在区间操作和可持久化中占优（给清华爷跪了）


```c++
// treap
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <ctime>

using namespace std;

const int MAXN = 100005;

struct Node {
    int l, r;      // 左右儿子
    int val, pri;  // 权值和随机优先级
    int cnt;       // 当前权值出现的次数（处理重复值）
    int size;      // 子树大小
} t[MAXN];

int n, root, tot;

// 更新子树大小
void update(int p) {
    t[p].size = t[t[p].l].size + t[t[p].r].size + t[p].cnt;
}

// 右旋 (Zig)
void zig(int &p) {
    int q = t[p].l;
    t[p].l = t[q].r;
    t[q].r = p;
    update(p); update(q);
    p = q;
}

// 左旋 (Zag)
void zag(int &p) {
    int q = t[p].r;
    t[p].r = t[q].l;
    t[q].l = p;
    update(p); update(q);
    p = q;
}

// 插入
void insert(int &p, int v) {
    if (!p) {
        p = ++tot;
        t[p].val = v;
        t[p].pri = rand(); // 随机优先级
        t[p].cnt = t[p].size = 1;
        return;
    }
    t[p].size++;
    if (t[p].val == v) {
        t[p].cnt++;
    } else if (v < t[p].val) {
        insert(t[p].l, v);
        if (t[t[p].l].pri > t[p].pri) zig(p); // 插入后检查堆性质
    } else {
        insert(t[p].r, v);
        if (t[t[p].r].pri > t[p].pri) zag(p);
    }
}

// 删除
void del(int &p, int v) {
    if (!p) return;
    if (t[p].val == v) {
        if (t[p].cnt > 1) {
            t[p].cnt--;
            t[p].size--;
        } else {
            // 将节点旋转到叶子或单分支位置再删除
            if (!t[p].l || !t[p].r) {
                p = t[p].l + t[p].r;
            } else if (t[t[p].l].pri > t[t[p].r].pri) {
                zig(p); del(t[p].r, v);
            } else {
                zag(p); del(t[p].l, v);
            }
        }
    } else if (v < t[p].val) {
        del(t[p].l, v);
    } else {
        del(t[p].r, v);
    }
    if (p) update(p);
}

// 查排名
int get_rank(int p, int v) {
    if (!p) return 0;
    if (t[p].val == v) return t[t[p].l].size + 1;
    if (v < t[p].val) return get_rank(t[p].l, v);
    return t[t[p].l].size + t[p].cnt + get_rank(t[p].r, v);
}

// 查第 k 大
int get_val(int p, int k) {
    if (!p) return 0;
    if (k <= t[t[p].l].size) return get_val(t[p].l, k);
    if (k <= t[t[p].l].size + t[p].cnt) return t[p].val;
    return get_val(t[p].r, k - t[t[p].l].size - t[p].cnt);
}

// 前驱
int get_pre(int p, int v) {
    if (!p) return -2e9; // 负无穷
    if (t[p].val >= v) return get_pre(t[p].l, v);
    return max(t[p].val, get_pre(t[p].r, v));
}

// 后继
int get_nxt(int p, int v) {
    if (!p) return 2e9; // 正无穷
    if (t[p].val <= v) return get_nxt(t[p].r, v);
    return min(t[p].val, get_nxt(t[p].l, v));
}
```

```c++
#include <iostream>
#include <random>

using namespace std;

const int MAXN = 100005;

struct Node {
    int l, r;       // 左右儿子编号
    int val, pri;   // val: BST 权值; pri: 堆随机优先级
    int size;       // 子树大小，用于计算排名
} t[MAXN];

int cnt, root;
mt19937 rnd(2026); // 使用更现代的随机数生成器，2026 为种子

// 创建一个新节点
int new_node(int v) {
    t[++cnt].val = v;
    t[cnt].pri = rnd(); // 随机分配优先级，保证平衡性
    t[cnt].size = 1;
    t[cnt].l = t[cnt].r = 0;
    return cnt;
}

// 向上更新节点信息（如 size）
void pushup(int p) {
    t[p].size = t[t[p].l].size + t[t[p].r].size + 1;
}

/**
 * 分裂 (Split)
 * 将根为 p 的树按权值 v 分成两棵树 x 和 y
 * x 树中的所有节点权值 <= v
 * y 树中的所有节点权值 > v
 */
void split(int p, int v, int &x, int &y) {
    if (!p) { x = y = 0; return; } // 递归边界
    
    if (t[p].val <= v) {
        // 当前节点及其左子树都属于 x
        x = p;
        // 递归处理右子树，看右子树里是否还有 <= v 的节点
        split(t[p].r, v, t[x].r, y);
    } else {
        // 当前节点及其右子树都属于 y
        y = p;
        // 递归处理左子树，看左子树里是否还有 > v 的节点
        split(t[p].l, v, x, t[y].l);
    }
    pushup(p); // 分裂后结构改变，需要更新 size
}

/**
 * 合并 (Merge)
 * 将根为 x 和 y 的两棵树合并，返回合并后的根
 * 前提：x 树中的最大权值 <= y 树中的最小权值
 */
int merge(int x, int y) {
    if (!x || !y) return x + y; // 如果有一棵树为空，直接返回另一棵
    
    // 根据随机优先级 pri 维护大根堆性质
    if (t[x].pri > t[y].pri) {
        // x 优先级更高，x 作为父节点
        // 因为 x < y，所以 y 应该在 x 的右子树里
        t[x].r = merge(t[x].r, y);
        pushup(x);
        return x;
    } else {
        // y 优先级更高，y 作为父节点
        // 因为 x < y，所以 x 应该在 y 的左子树里
        t[y].l = merge(x, t[y].l);
        pushup(y);
        return y;
    }
}

// 插入一个权值为 v 的数
void insert(int v) {
    int x, y;
    split(root, v, x, y); // 按 v 分裂成 <=v 和 >v
    root = merge(merge(x, new_node(v)), y); // 重新按顺序合回去
}

// 删除一个权值为 v 的数
void del(int v) {
    int x, y, z;
    split(root, v, x, z);      // 分裂成 <=v (x) 和 >v (z)
    split(x, v - 1, x, y);     // 在 x 中进一步分裂成 <v (x) 和 ==v (y)
    if (y) {
        // y 里的节点全都是 v。我们只删除一个，所以合并 y 的左右儿子
        y = merge(t[y].l, t[y].r);
    }
    root = merge(merge(x, y), z); // 合并回去
}

// 查询权值 v 的排名
int get_rank(int v) {
    int x, y;
    split(root, v - 1, x, y); // 分裂出 <v 的部分
    int res = t[x].size + 1;  // 排名即为比它小的个数 + 1
    root = merge(x, y);       // 别忘了合并还原
    return res;
}

// 查询排名为 k 的权值
int get_val(int p, int k) {
    while (true) {
        if (k <= t[t[p].l].size) {
            p = t[p].l; // 在左子树
        } else if (k == t[t[p].l].size + 1) {
            return t[p].val; // 就是当前根节点
        } else {
            // 在右子树，排名要扣掉左边和根的大小
            k -= (t[t[p].l].size + 1);
            p = t[p].r;
        }
    }
}

// 求 v 的前驱（小于 v 的最大值）
int get_pre(int v) {
    int x, y;
    split(root, v - 1, x, y); // 分裂出 <v 的树 x
    int res = get_val(x, t[x].size); // x 中最大的那个
    root = merge(x, y);
    return res;
}

// 求 v 的后继（大于 v 的最小值）
int get_nxt(int v) {
    int x, y;
    split(root, v, x, y); // 分裂出 >v 的树 y
    int res = get_val(y, 1); // y 中最小的那个
    root = merge(x, y);
    return res;
}

int main() {
    // 使用示例
    insert(10);
    insert(20);
    insert(5);
    printf("5 的排名: %d\n", get_rank(5));    // 输出 1
    printf("排名为 2 的值: %d\n", get_val(root, 2)); // 输出 10
    return 0;
}
```

## 3 常用算法模板

- 递归、贪心、分治这种更偏向于算法思想的就不列举了

### 3.0 快读

- 读入速度：fread() > scanf > cin

```c++
inline int fread(){
    int x = 0, f = 1;
    char ch = getchar();
    while(ch<'0' || ch>'9'){
        if(ch == '-'){
            f = -1;
        }
        ch = getchar();
    }
    while(ch>='0' && ch<='9'){
        x = (x<<3)+(x<<1)+(ch^48); // x*8+x*2+ch-'0'
        ch = getchar();
    }
    return x*f;
}
```

### 3.1 搜索

#### 3.1.0 dfs

- 深度优先搜索，本质递归
- 注意退出条件和标记回溯
- 小心爆栈内存

```c++
void dfs(int depth){
    if(exp){	// 满足条件
        // 某种操作
        return;
    }
    mark[depth] = true;
    dfs(depth+1);
    mark[depth] = false;
}
```

#### 3.1.1 bfs

- 广度优先搜索，通过队列的性质来保证“广度优先”
- 求出的路径是最短路径

```c++
// 伪代码
void bfs(){
    int head = 0, tail = 0;
    int q[1000];
    q[tail++] = start_node; // 初始节点入队
    while(head < tail){ // 队列不空
        for(item in q[head]的所有后继情况){
            if(item_available){
                q[tail++] = item; // 满足条件的后继入队，等待被搜索
                item_available = false;
            }
        }
        head++; // 队首节点的所有节点都被遍历，没用了，就出队
	}
}

// bfs怎么输出搜索路径？（考虑AI基础第二次作业的T2附加题）
struct Node{
    int data;
    int father;
}Node;
// 入队时
q[tail].data = item.data;
q[tail++].father = head; // 存前驱节点下标
// 当找到终点时，一直回溯前驱直到找到起始位置，然后逆向输出
```

### 3.2 排序

#### 3.2.0 归并

- 用来求逆序对

```c++
#include <iostream>
#include <vector>

using namespace std;

typedef long long ll;
const int MAXN = 500005;

int a[MAXN], tmp[MAXN];
ll ans = 0; // 存储逆序对总数

void merge_sort(int l, int r) {
    if (l >= r) return;

    int mid = (l + r) >> 1;
    merge_sort(l, mid);       // 递归左半部分
    merge_sort(mid + 1, r);   // 递归右半部分

    // 合并过程
    int i = l, j = mid + 1, k = l;
    while (i <= mid && j <= r) {
        if (a[i] <= a[j]) {
            tmp[k++] = a[i++];
        } else {
            // 核心计数：a[i] > a[j]，则 a[i...mid] 均大于 a[j]
            ans += (mid - i + 1);
            tmp[k++] = a[j++];
        }
    }

    // 处理剩余元素
    while (i <= mid) tmp[k++] = a[i++];
    while (j <= r) tmp[k++] = a[j++];

    // 写回原数组
    for (int p = l; p <= r; p++) a[p] = tmp[p];
}

int main() {
    int n;
    if (scanf("%d", &n) != 1) return 0;
    for (int i = 1; i <= n; i++) scanf("%d", &a[i]);

    merge_sort(1, n);

    printf("%lld\n", ans);
    return 0;
}
```

#### 3.2.1 快排

- 学过计科导的没道理不会了
- 直接用STL的sort完事了（真的有人C++手写qsort吗）

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

void quickSort(int a[], int l, int r) {
    if (l >= r) return;

    // 1. 确定分界点：通常取中间值，防止顺序数据退化
    int x = a[(l + r) >> 1]; 
    int i = l - 1, j = r + 1;

    // 2. 调整区间：左边全 <= x，右边全 >= x
    while (i < j) {
        do i++; while (a[i] < x);
        do j--; while (a[j] > x);
        if (i < j) swap(a[i], a[j]);
    }

    // 3. 递归处理左右两个子区间
    quickSort(a, l, j);
    quickSort(a, j + 1, r);
}

int main() {
    int n;
    scanf("%d", &n);
    vector<int> a(n);
    for (int i = 0; i < n; i++) scanf("%d", &a[i]);

    quickSort(a.data(), 0, n - 1);

    for (int i = 0; i < n; i++) printf("%d ", a[i]);
    return 0;
}
```

#### 3.2.2 冒泡

```c++
for(int i=0; i<n-1; ++i){
    swapped = true;
    for(int j=0; j<n-i-1; ++j){
        if(a[j] < a[j+1]){
            swap(a[j], a[j+1]);
            swapped = true;
        }
    }
    if(!swapped){
        break;
    }
}
```

#### 3.2.3 桶排

- 空间换时间的典型
- 在数据很稀疏的情况下效率并不高

```c++
int arr[n]; // n为数据的个数
int t[N]; // N为数据的最大值
memset(t, 0, sizeof(t));
int maxn = -0x7fffffff;
for(int i=0; i<n; ++i){
    t[arr[i]]++;
    maxn = max(maxn, arr[i]);
}
for(int i=0; i<maxn; ++i){
    if(t[i]){
        printf("%d ", i); // 升序排序输出
    }
}
```

### 3.3 字符串相关

- 高精度运算说白了就是模拟列竖式，纯字符串处理（大模拟这一块）
  - 也可以用vector


#### 3.3.0 模式串匹配：KMP&哈希

- KMP上课讲过了

```c++
//KMP
void get_next(){
    next[1] = 0;
    int j = 0;
    for(int i=2; i<=m; ++i){
        while(j && sub[j+1]!=sub[i]){
            j = next[j];
		}
        if(sub[j+1] == sub[i]){
            ++j;
        }
        next[i] = j;
	}
}
//路径压缩的写法
void get_nextval(){
    nextval[1] = 0;
    int j = 0;
    for(int i=2; i<=m; ++i){
        while(j && sub[j+1]!=sub[i]){
            j = nextval[j];
		}
        if(sub[j+1] == sub[i]){
            ++j;
        }
        if(j>0 && sub[i+1]==sub[j+1]){
            nextval[i] = nextval[j];
        }else{
            nextval[i] = j;
        }
	}
}
void kmp(){
    get_next();
    for(int i=1, j=0; i<=n; ++i){
        while(j && s[i]!=sub[j+1]){
            j = next[j];
        }
        if(s[i] == p[j+1]){
            ++j;
		}
        if(j == m){
            cout << i-m+1;
            j = next[j];
		}
	}
}
```

- 哈希是把字符串通过看成一个base进制数，建立字符串->数的映射
  - 若字符串中有k种字符，那么base应该取一个大于k的质数
    - base一般取131或者13331
  - 取模的M一般取$10^9+7$或者$10^9+9$；也可以将hash设置为`unsigned long long`，溢出对$2^{64}$自动取模

```c++
void init(int n, char str[]) {
    power[0] = 1;
    for(int i=1; i<=n; ++i){
        power[i] = power[i-1]*base; // power[i]是base^i
        hash[i] = hash[i-1]*base+str[i]; // hash[i]存的是[1,i]区间中子串的哈希值
        									// 省略了取模
    }
}
typedef unsigned long long ull;
// 获取子串[l, r]的哈希值
ull get_hash(int l, int r) {
    return hash[r]-hash[l-1]*power[r-l+1]; 
    // 如果要取模，建议写为：
    // ((hash[r]%m)>((hash[l-1]*power[r-l+1])%m))?(((hash[r]%m)-((hash[l-1]*power[r-l+1])%m))%m):(((hash[r]%m)+m-((hash[l-1]*power[r-l+1])%m))%m)
    // 即注意正负号的问题，如果小于则先加mod再计算
}

// 匹配时，求出匹配串的值，之后用滑动窗口扫一遍主串即可
```

#### 3.3.1 Trie字典树

- 字符存在树的边上
- 从根到结束节点的路径构成了字符串

```c++
int trie[N][26]; // 每个节点可以引申出26条边，对应'a'-'z'
int cnt[N]; // 以节点n结尾的字符串数量
int idx; // 树的节点编号
void insert(char str[]){
    int p = 0;
    for(int i=0; i<strlen(str); ++i){
        int u = str[i]-'a';
        if(!trie[p][u]){
            trie[p][u] = ++idx; // 如果不存在路径，就新建一个对应该字符的路径并指向新节点
		}
        p = trie[p][u];
    }
    cnt[p]++; // 字符串结束，标记该节点为结束节点
}
int find(char str[]){
    int p = 0;
    for(int i=0; i<strlen(str); ++i){
        int u = str[i]-'a';
        if(!trie[p][u]){
            return -1; // 没找到
        }
        p = trie[p][u];
	}
    return cnt[p]; // 返回找到的字符串个数
}
```

#### 3.3.2 AC自动机

- 可以自动AC的神秘自动机（bushi

- 处理多模式串匹配
  - 构造模式串的Trie
  - 对Trie上的节点构造前缀指针
  - 利用前缀指针对主串进行匹配
- 可以看做KMP+Trie，是一棵树上的KMP

```c++
// 建立trie
void make_trie(char s[]){
    int p=0;
    for(int i=0; i<strlen(s); ++i){
        int u = s[i]-'a';
        if(!trie[p][u]){
            trie[p][u] = ++idx;
        }
        p = trie[p][u];
	}
    cnt[p]++;
    return;
}

// bfs建立前缀指针nxt
void bfs(){
    queue<int> q;
    for(int i=0; i<26; ++i){
        if(trie[0][i]){
            q.push(trie[0][i]); // 预处理根节点
		}
    }
    while(!q.empty()){ // q也可以用数组实现
        int u = q.front();
        q.pop();
        for(int i=0; i<26; ++i){
            if(!trie[u][i]){
                trie[u][i] = trie[nxt[u]][i]; // 路径压缩，参考KMP的nextval
            }else{
                q.push(trie[u][i]); // 节点存在则入队
                nxt[trie[u][i]] = trie[nxt[u]][i];
            }
		}
    }
}

int query(){
    int p = 0;
    for(int i=0; i<strlen(s); ++i){ // s是主串
        p = trie[p][s[i]-'a'];
        for(int j=p; j; j=nxt[j]){ // 向上寻找路径是否有单词结尾
            if(cnt[j]){
                return 1; // 如果是统计子串个数，在本次统计后将cnt[j]置为-1，避免重复
			}
        }
    }
    return 0;
}
```

### 3.4 图论

#### 3.4.0 最短路

- 除了Floyd都是单源最短路
- E是边集，V是点集，n是点数，m是边数

##### 3.4.0.0 Floyd-Warshall

- O($n^3$)
- 可以求出任何两点之间的最短路
  - 多源最短路才用
  - 暴力枚举所有中转点
- 用邻接矩阵存
- 可以有负权边，**不能**有**负环**，如果有那么将无限向下更新（负环每转一圈权和都会变小）

```c++
for(int k=1; k<=n; ++k){
    for(int i=1; i<=n; ++i){
        for(int j=1; j<=n; ++j){
            if(d[i][k]!=INF && d[k][j]!=INF){ // INF在limits库中，可以手写0x7fffffff
                d[i][j] = min(d[i][j], d[i][k]+d[k][j]);
			}
        }
    }
}
```

##### 3.4.0.1 Dijkstra+堆优化

- 示例来自gemini，它把堆的操作做了融合，本质和2.5是一样的

- 大家都学过dijkstra，只给出优化的版本，O(ElogV)
- 贪心的思路，每次处理距离最小的那一个
  - 维护一个小根堆来优化
- **不能**处理**负权边**
  - 没有负权边且是单源最短路优先使用
- 邻接表存

```c++
// 堆节点结构体
struct HeapNode {
    int d; // 到起点的距离 (排序依据)
    int u; // 节点编号
};

HeapNode heap[M]; // 堆数组，大小开边数 M (因为最坏情况下每条边都可能引发一次 push)
int heap_sz;      // 堆中当前元素个数

// 交换堆中两个节点
void swap_node(int i, int j) {
    HeapNode temp = heap[i];
    heap[i] = heap[j];
    heap[j] = temp;
}

// 往堆中插入元素 (向上调整 / Swim)
void push(int d, int u) {
    heap[++heap_sz] = {d, u}; // 放到堆的最后
    int curr = heap_sz;
    
    // 与父节点 (curr / 2) 比较，如果比父节点小，则上浮
    while (curr > 1) {
        int p = curr / 2;
        if (heap[curr].d < heap[p].d) {
            swap_node(curr, p);
            curr = p; // 继续向上
        } else {
            break; // 已经满足小根堆性质
        }
    }
}

// 弹出堆顶最小元素 (向下调整 / Sink)
void pop() {
    heap[1] = heap[heap_sz--]; // 将堆尾元素移动到堆顶，并缩小堆大小
    int curr = 1;
    
    // 不断与较小的子节点交换下沉
    while (curr * 2 <= heap_sz) { // 只要还有左儿子
        int child = curr * 2;     // 左儿子
        
        // 如果有右儿子，且右儿子比左儿子更小，我们就指向右儿子
        if (child + 1 <= heap_sz && heap[child + 1].d < heap[child].d) {
            child++;
        }
        
        // 如果当前节点比最小的子节点大，则下沉
        if (heap[curr].d > heap[child].d) {
            swap_node(curr, child);
            curr = child;
        } else {
            break; // 已经满足小根堆性质
        }
    }
}

bool vis[N]; // 记录节点的最短路是否已经确定

void dijkstra(int start, int n) {
    // 1. 初始化
    for (int i = 1; i <= n; ++i) {
        dist[i] = INF;
        vis[i] = false;
    }
    heap_sz = 0; // 清空堆
    
    dist[start] = 0;
    push(0, start); // 起点入堆
    
    // 2. 开始扩展
    while (heap_sz > 0) {
        // 取出堆顶 (当前距离最小的节点)
        int u = heap[1].u;
        pop(); // 弹出堆顶
        
        if (vis[u]) continue; // 核心：如果该点已经出堆过，说明它的最短路已确定，直接废弃
        vis[u] = true;
        
        // 遍历相邻边
        for (int i = head[u]; i != 0; i = nxt[i]) {
            int v = to[i];
            int w = weight[i];
            
            if (dist[v] > dist[u] + w) {
                dist[v] = dist[u] + w;
                push(dist[v], v); // 将更新后的距离入堆
            }
        }
    }
}
```

##### 3.4.0.2 Bellman-Ford

- 可以检测负环
- 用于处理“最多经过k条边”的问题
- 边集数组存

```c++
#define INF 0x7fffffff
struct Edge{
    int u, v, w;
}edges[M];

int dist[N], backup[N]; // dist存源到目的的最短路权
void bellman_ford(int st, int k){ // 如果不限制路径数目，k=n即可
    for(int i=0; i<n; ++i){
        dist[i] = INF; // 初始化为INF
	}
    dist[st] = 0; // 起点到自己的路径为0
    for(int i=0; i<k; ++i){
        memcpy(backup, dist, sizeof(dist)); // 确保每一轮循环只新增一条边
        for(int j=0; j<m; ++j){ // 说白了还是枚举中转边
            int u = edges[j].u, v = edges[j].v, w = edges[v].w;
            if(backup[u]!=INF && backup[u]+w<dist[v]){
                dist[v] = backup[u]+w;
			}
		}
	}
}
```

##### 3.4.0.3 SPFA

- Bellman-Ford的队列优化
  - 在检测负环、处理负权边时选用
- 邻接表存
- 不想手写可以queue+vector实现

```c++
int dist[N];
bool book[N]; // 打标记，避免重复入队
int q[N+5];

for(int i=1; i<=n; ++i){
    dist[i] = INF;
    book[i] = 0;
}
dist[start] = 0;
int head = 0, tail = 0;
q[tail++] = start;
book[start] = 1;

while(head != tail){ // 使用循环队列，head、tail不能用<判空
    int u = q[head++];
    if(head == N){
        head = 0; // 为什么不取模？除运算(div)很慢，而cmp和jmp很快
	}
    book[u] = 0;
    for(int i=head[u]; i!=-1; i=next[i]){
        int v = to[i];
        int w = weight[i];
        if(dist[v] > dist[u]+w){
            dist[v] = dist[u]+w; // 松弛边
            if(!book[v]){ // 如果当前点可以作为中转且没有入队，则入队
                q[tail++] = v;
                if(tail == N){
                    tail = 0; 
				}
                book[v] = 1;
            }
		}
	}
}
```

#### 3.4.1 拓扑排序

- 只能处理DAG有向无环图

- 处理有顺序依赖关系的排序
- 邻接矩阵或者邻接表都行

```c++
int in_degree[N]; // 记录每个节点的入度
int topo_seq[N];  // 记录拓扑排序的结果
int topo_cnt = 0; // 结果序列的长度

bool kahn(int n) {
    int q[N], head = 0, tail = 0;
    for(int i=1; i<=n; ++i){
        if(in_degree[i] == 0){ // 入度为0，没有前置依赖，可以直接作为起点开始遍历
            q[tail++] = i;
        }
    }
    while(head < tail){
        int u = q[head++];
        topo_seq[++topo_cnt] = u; // 记录到答案序列中
        for(int i=head[u]; i!=-1; i=next[i]){
            int v = to[i];
            in_degree[v]--; // 节点u已经遍历完，从图中删除u出发的所有边，即v的入度-1
            // 如果v的前置依赖全部完成，入队
            if(in_degree[v] == 0){
                q[tail++] = v;
            }
        }
    }
    // 如果加入序列的节点数等于 n，说明是 DAG；否则有环
    return topo_cnt == n; 
}
```

#### 3.4.2 最小生成树

- n个点，显然可以用n-1条边连起来，形成一棵树
  - 在图G{V, E}中，从E中选择|V|-1条边来形成一棵树，并且使得这棵树的边权最小，就是求最小生成树
- 贪心思路求解
  - 提供两种算法模板
  - Prim适合稠密图，Kruskal适合稀疏图

```c++
// Prim
// 邻接矩阵存
// 好像可以堆优化来着，但我忘记怎么写了（
int g[n][n];
int visit[n]; // 标记顶点是否加入最小生成树
int d[n]; // d[i]为节点i与生成树中点最小的边长
void Prim(int v0){ // v0可以是任意一个节点，作为生成树的起始
    memset(visit, 0, sizeof(visit));
    for(int i=1; i<=n; ++i){
        d[i] = INF;
	}
    d[v0] = 0;
    int ans = 0, k;
    for(int i=1; i<=n; ++i){
        int minn = INF;
        for(int j=1; j<=n; ++j){
            if(!visit[j] && minn>d[j]){
                minn = d[j]; // 贪心找树外和树相连的最小边
                k = j;
			}
		}
        visit[k] = 1;
        ans += d[k];
        for(int j=1; j<=n; ++j){
            if(!visit[j] && d[j]>g[k][j]){ // 对距离做松弛
                d[j] = g[k][j];
			}
        }
    }
}
```

```c++
// Kruskal
struct Edge{
    int u, v, w;
}a[n];
int prt[n];
bool cmp(const Edge& a, const Edge& b){
    return a.w<b.w;
}
int get_father(int x){ // 并查集
    if(prt[x] == x){
        return x;
	}
    prt[x] = get_father(prt[x]);
    return prt[x];
}
int kruskal(){
    sort(a+1, a+m+1, cmp); // 按权升序
    int f1, f2;
    int k = 0, ans = 0;
    for(int i=1; i<=n; ++i){
        prt[i] = i;
	}
    for(int i=1; i<=m; ++i){ // 贪心，优先选择不在同一集合的两点之间的、权更小的边
        f1 = get_father(a[i].u);
        f2 = get_father(a[i].v);
        if(f1 != f2){
            ans += a[i].w;
            prt[f1] = f2; // 更新并查集
            ++k;
            if(k == n-1){ // 如果n-1条边，则不用继续找了
                break;
            }
		}
    }
    if(k == n-1){
        return ans;
    }
    return -1;
}
```

#### 3.4.3 倍增求LCA

- 示例是gemini写的
- 找公共祖先如果一步一步上跳会很慢，考虑每次走$2^k$，即倍增

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

const int MAXN = 500005; // 最大节点数
const int MAXLOG = 20;   // 2^20 足够覆盖 5e5 的深度 (log2(5e5) ≈ 19)

vector<int> adj[MAXN];    // 邻接表存图
int depth[MAXN];          // 节点的深度
int fa[MAXN][MAXLOG];     // fa[i][j] 表示节点 i 的第 2^j 个祖先

// 1. DFS 预处理：计算深度和初始祖先 (2^0)
void dfs(int u, int p) {
    depth[u] = depth[p] + 1;
    fa[u][0] = p; // 2^0 级祖先就是父节点

    // 预处理 2^k 级祖先
    for (int k = 1; k < MAXLOG; k++) {
        fa[u][k] = fa[fa[u][k - 1]][k - 1];
    }

    // 递归处理子节点
    for (int v : adj[u]) {
        if (v != p) {
            dfs(v, u);
        }
    }
}

// 2. 查询 LCA
int getLCA(int u, int v) {
    // 确保 u 的深度不小于 v
    if (depth[u] < depth[v]) swap(u, v);

    // 第一步：将 u 提升到与 v 同一高度
    // 利用位运算，从大到小尝试跳跃
    for (int k = MAXLOG - 1; k >= 0; k--) {
        if (depth[fa[u][k]] >= depth[v]) {
            u = fa[u][k];
        }
    }

    // 如果对齐后 u 和 v 重合，说明 v 是 u 的祖先
    if (u == v) return u;

    // 第二步：u 和 v 同时向上跳
    // 目标：跳到 LCA 的下一层
    for (int k = MAXLOG - 1; k >= 0; k--) {
        // 如果跳 2^k 步后两个点不重合，就往上跳
        // 这样可以保证最后停在 LCA 的正下方
        if (fa[u][k] != fa[v][k]) {
            u = fa[u][k];
            v = fa[v][k];
        }
    }

    // 返回此时 u 的父节点，即为 LCA
    return fa[u][0];
}

int main() {
    int n, m, root;
    // n 节点数, m 查询数, root 根节点编号
    scanf("%d %d %d", &n, &m, &root);

    for (int i = 0; i < n - 1; i++) {
        int u, v;
        scanf("%d %d", &u, &v);
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    // 预处理
    // 技巧：设 fa[root][0] = 0，depth[0] = 0，避免越界判断
    dfs(root, 0);

    for (int i = 0; i < m; i++) {
        int a, b;
        scanf("%d %d", &a, &b);
        printf("%d\n", getLCA(a, b));
    }

    return 0;
}
```

#### 3.4.4 二分图

- 二分图：可以将顶点分成两个集合，使得每条边的两个端点属于不同的集合
- 染色法：如果节点k染色为颜色A，则它的邻居只能染色为颜色B

```c++
int color[MAXN]; // 0: 未染色, 1: 颜色A, 2: 颜色B
vector<int> adj[maxn];

// 返回 true 表示以 u 为起点的连通分量是二分图
bool dfs_color(int u, int c){
    color[u] = c; // 给当前点染色
    for(int v: adj[u]){
        if(!color[v]){ // 邻居没染色
            if(!dfs_color(v, 3-c)){
                return false; // 染相反色 (3-1=2, 3-2=1)
            }
        }else if(color[v] == c){ // 邻居已染色且颜色冲突
            return false;
        }
    }
    return true;
}

// 主函数调用逻辑
bool is_bipartite(int n){
    for(int i=1; i<=n; ++i){
        if(!color[i]){ // 整个图可能是多个不连通的子图，分别处理
            if(!dfs_color(i, 1)){
                 return false;
            }
        }
    }
    return true;
}
```

### 3.5 动态规划

- 将问题拆分成子问题结构解决（有点像递归），并且子问题满足无后效性
  - 当前的解决方案不会影响之后的决策

- 我真的不会推转移方程QAQ

#### 3.5.0 线性DP

- 递推方程是线性的

- 最长上升序列：`dp[i] = max(dp[i], dp[j]+1) 当 j<i&&a[j]<a[i]`

  - dp[i]是第i个字符结尾的最长上升序列长度

  - ```c++
    int a[MAXN], dp[MAXN];
    
    int solve(int n){
        int ans = 0;
        for (int i=1; i<=n; ++i) {
            dp[i] = 1; // 初始状态：每个数自成一个序列，长度为 1
            for(int j=1; j<i; ++j){
                if(a[j] < a[i]){
                    dp[i] = max(dp[i], dp[j]+1);
                }
            }
            ans = max(ans, dp[i]); // 全局最大值即为答案
        }
        return ans;
    }
    ```

- 最长公共前缀：`dp[i][j]`为A序列前i字符和B序列前j字符的最长公共前缀大小

  - `dp[i][j] = dp[i-1][j-1]+1 当 A[i]==B[j]`
  - `dp[i][j] = max(dp[i-1][j], dp[i][j-1]) 当 A[i]!=B[j]`

#### 3.5.1 背包DP

- 现在有一个背包，容量固定，有n个物品，第i个物品消耗容量`w[i]`，价值为`v[i]`，怎么拿价值最高？

- `dp[i][j]`表示前i个物品放入容量为j的背包所能获得的最大价值

- 0/1背包

  - 每个物品只有一件，要么选要么不选

  - `dp[i][j] = max(dp[i-1][j], dp[i-1][j-w[i]]+v[i])`，分别对应不选/选的状态

  - ```c++
    for(int i=1; i<=n; ++i){
        for(int j=m; j>=w[i]; --j){ // 逆序，要装w[i]的物品，容量至少是w[i]
            dp[j] = max(dp[j], dp[j-w[i]]+v[i]); // 省略第一维的循环
        }
    }
    ```

- 完全背包

  - 每种物品有无限个

  - 和0/1背包很像，看方程自己悟（因为我说不清楚）

  - 尝试说明：选择第i个物品仍然从第i行转移是因为物品无限

  - `dp[i][j] = max(dp[i-1][j], dp[i][j-w[i]]+v[i])`

  - ```c++
    for(int i=1; i<=n; ++i){
        for(int j=w[i]; j<=m; ++j){ // 正序遍历代表每件物品可选多次
            dp[j] = max(dp[j], dp[j-w[i]]+v[i]);
        }
    }
    ```

- 多重背包

  - 第i个物品有s[i]个

  - 在0/1背包基础上多加一层循环，枚举选几件

  - ```c++
    for(int i=1; i<=n; ++i){           		// 1. 遍历物品
    	for(int j=m; j>=w[i]; --j){    		// 2. 逆序遍历容量（类似 0/1 背包）
    										// 3. 决策：枚举选 k 个当前物品
    		for(int k=1; k<=s[i] && k*w[i]<=j; ++k) {
    			dp[j] = max(dp[j], dp[j-k*w[i]]+k*v[i]);
    		}
    	}
    }
    ```

#### 3.5.2 区间DP

- `dp[i][j]`表示区间[i, j]中的最优解
- $dp[i][j] = \min_{i \le k < j} \{ dp[i][k] + dp[k+1][j] + \text{cost}(i, j) \}$
- 求解过程：
  - 枚举区间长度，枚举左端点，再枚举区间断点进行转移

```c++
// 1. 初始化 dp 数组（如果是求最小值，初始化为无穷大；求最大值则初始化为 0 或负无穷）
memset(dp, 0x3f, sizeof(dp));

// 2. 初始化基础状态（长度为 1 的区间）
for (int i = 1; i <= n; i++) {
    dp[i][i] = 0; // 单个元素合并代价通常为 0，具体视题目而定
}

// 3. 核心转移逻辑
for (int len = 2; len <= n; len++) {           // 第一层：枚举区间长度，从 2 逐渐扩展到 n
    for (int i = 1; i <= n - len + 1; i++) {   // 第二层：枚举区间的起点 i
        int j = i + len - 1;                   // 根据起点和长度算出区间的终点 j
        for (int k = i; k < j; k++) {          // 第三层：枚举区间 [i, j] 的分割点 k
            // 状态转移方程
            dp[i][j] = min(dp[i][j], dp[i][k] + dp[k+1][j] + cost(i, j));
        }
    }
}
// 最终答案通常在 dp[1][n] 中
```

- 经典例题：[P1775 石子合并（弱化版） - 洛谷](https://www.luogu.com.cn/problem/P1775)
- 对于成环的区间DP，考虑将数据复制一份接在末尾，形成一组2n大小的数据
  - 这样可以模拟成环时任意起点所构成的序列（断环成链）
  - 状态转移方程不变
  - 最终答案为$\min_{1 \le i \le n} \{ dp[i][i+n-1] \}$

#### 3.5.3 树形DP

- 在树上的dp
  - 没有线性结构，只能用dfs来求解（遍历子节点后来推出根节点，即后序遍历）
- 例题：给一棵树，每个节点有一个对应的权R[i]，规定对于任意一条边，至多能选择相连的一个节点；求如何选节点，使得权最大？
  - `dp[u][1]`表示选择u时的最优解，`dp[u][0]`为不选择u时的最优解
  - 若v为u的儿子，则$dp[u][0] = \sum \max(dp[v][0], dp[v][1])$，$dp[u][1] = R[u] + \sum dp[v][0]$

```c++
void dfs(int u){
    dp[u][0] = 0;
    dp[u][1] = R[u];
    for(int v: adj[u]){ // adj为邻接表，vector<int> adj[maxn];
        dfs(v);
        dp[u][0] += max(dp[v][0], dp[v][1]);
        dp[u][1] += dp[v][0];
    }
    return;
}
```

#### 3.5.4 状态压缩DP

- 将n个“取/不取”即0/1的状态看做一个n位二进制数，将n维的状态压缩为1维
- 位运算操作：
  - `state&(1<<i)`判定第i位是否是1
  - `state |= (1<<i)` 将第i位置1
  - `state &= ~(1<<i)`将第i位置0
  - `state ^= (1<<i)`翻转第i位
  - `stateA & stateB!=0`判定两个状态有无交集
  - `for(int sub=state; sub>0; sub=(sub-1)&state){ // 处理子集 sub }`遍历子集
- 状压dp的复杂度很高，一般处理$n \le 20$
- 例题：旅行商问题：从城市0出发，恰好经过n-1个城市后回到城市0，要求经过的路径最短

```c++
// 初始化 DP 数组为无穷大
memset(dp, 0x3f, sizeof(dp));
// base case: 只访问了城市 0，且当前在城市 0，距离为 0
dp[1][0] = 0; 

int total_states = 1 << n;
// 枚举所有状态
for (int state = 1; state < total_states; state++) {
    // 枚举当前所在的城市 u
    for (int u = 0; u < n; u++) {
        // 如果当前状态 state 中根本没有包含城市 u，直接跳过
        if (!((state >> u) & 1)) continue;
        
        // 枚举上一步所在的城市 v
        for (int v = 0; v < n; v++) {
            // 如果 u 和 v 是同一个城市，或者 state 中没有城市 v，跳过
            if (u == v || !((state >> v) & 1)) continue;
            
            // 状态转移
            int prev_state = state ^ (1 << u); 
            dp[state][u] = min(dp[state][u], dp[prev_state][v] + dist[v][u]);
        }
    }
}

// 最终答案：访问了所有城市（状态为 (1<<n)-1），从各个终点 u 回到 0 的最小代价
int ans = INF;
for (int u = 1; u < n; u++) {
    ans = min(ans, dp[total_states - 1][u] + dist[u][0]);
}
```

#### 3.5.5 数位DP

- 与数位相关，将复杂度变为O($log_{10}N$*状态数)
- 使用记忆dfs进行搜索，结果存在solve(x)中，即[L,R]中的解可表示为solve(R)-solve(L-1)
- 参数：
  - pos：当前位数
  - state：当前状态，通常与之前填的数相关
  - limit：有无限制
  - lead：前导0标志，比如枚举三位数填数，可能出现**099**的形式，根据题目可能需要特判前导0

- 例：[L, R]中有多少数满足：不包含4且不包含连续的62

```c++
#include <iostream>
#include <vector>
#include <cstring>
using namespace std;

// dp[pos][pre]：还剩 pos 位要填，且上一位数字是 pre 时，没有限制情况下的合法方案数
int dp[20][10]; 
int a[20]; // 存放拆分后的数字上限

// pos: 当前处理到第几位
// pre: 上一位填的数字
// limit: 是否受上限约束
int dfs(int pos, int pre, bool limit) {
    // 1. 边界条件：所有位都填完了，说明找到了一个合法数字，返回 1
    if (pos == 0) return 1; 
    
    // 2. 记忆化：如果不受限，且当前状态已经计算过，直接返回查表结果
    if (!limit && dp[pos][pre] != -1) {
        return dp[pos][pre];
    }
    
    // 3. 确定当前位能填的最高数字
    int up = limit ? a[pos] : 9;
    int ans = 0;
    
    // 4. 枚举当前位填数字 i
    for (int i = 0; i <= up; i++) {
        // 剪枝 1：不能包含 4
        if (i == 4) continue;
        // 剪枝 2：不能包含连续的 62
        if (pre == 6 && i == 2) continue;
        
        // 递归进入下一位：
        // 新的上一位变成了当前的 i
        // 只有在当前位受限，且填了最大值时，下一位才会受限
        ans += dfs(pos - 1, i, limit && (i == a[pos]));
    }
    
    // 5. 记录答案（只有在没有限制的情况下才记录通用解）
    if (!limit) {
        dp[pos][pre] = ans;
    }
    
    return ans;
}

// 拆分数字并启动搜索
int solve(int x) {
    int pos = 0;
    // 将数字 x 的每一位拆分存入数组 a 中，低位在低索引，高位在高索引
    while (x) {
        a[++pos] = x % 10;
        x /= 10;
    }
    // 从最高位 pos 开始递归
    // 初始状态 pre 传入 0 即可（因为 0 不影响后续判断）
    // 最高位一定受 limit 限制
    return dfs(pos, 0, true);
}

int main() {
    // dp 数组作为通用状态，整个程序运行期间只需要初始化一次！
    memset(dp, -1, sizeof(dp)); 
    
    int L = 1, R = 1000000;
    // 利用前缀和思想，求 [L, R] 等价于求 [0, R] - [0, L-1]
    cout << solve(R) - solve(L - 1) << endl;
    
    return 0;
}
```

### 3.6 数论

#### 3.6.0 快速幂

- 计算$a^n (mod m)$
  - 如果n为偶数，$a^n=(a^\frac{n}{2})^2$
  - 如果n为奇数，$a^n = a*a^{n-1}$

```c++
long long qpow(long long a, long long n, long long m){
    long long res = 1;
    a %= m;
    while(n > 0){
        if(n & 1){
            res = res*a%m;
		}
        a = a*a%m;
        n >>= 1;
    }
    return res%m;
}
```

#### 3.6.1 质数

- 只介绍欧拉筛（线性筛）

```c++
void get_primes(int n){
    is_not_prime[0] = is_not_prime[1] = true; // 0 和 1 不是质数
    for(int i=2; i<=n; ++i){
        if(!is_not_prime[i]){
            prime[++cnt] = i; // i 是质数，加入列表
        }
        for(int j=1; j<=cnt &&i*prime[j]<=n; ++j) {
            is_not_prime[i*prime[j]] = true; // 筛掉合数
            if(i%prime[j] == 0){
                break; 
            }
        }
    }
}
```



