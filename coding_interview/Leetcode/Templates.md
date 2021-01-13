## Tools

### Math

```python
import math
math.ceil(p/k) #equivalent to:
((p-1) // k) + 1 #two times faster
math.floor(p/k) #equivalent to:
p//k
```

### maxint minint

```python
import sys
max = sys.maxsize
min = -sys.maxsize - 1
```

### Reverse Range:

```python
for i in reversed(range(k)):
```



### Count then Sort by Count

```python
import collections
import heapq

count = list(collections.Counter(items).items())
#sort by count (max) then alphabetical
count.sort(key=lambda x: (-x[1], x[0]))
#heap to find top k count (logk time)
heapq.nlargest(k,count, lambda x: (-x[1], x[0]))
```

### Python Heapq

+ Only supports minHeap
+ Use `-value` for maxheap. 

```python
import heapq
list=[]
heap = heapq.heapify(list)
heapq.heappush(heap, item)
item = heapq.heappop()
size=len(heap)
peakItem = heap[0]
```



### Bisect

```python
bisect.bisect_left(a, x, lo=0, hi=len(a))
bisect.bisect_right(a, x, lo=0, hi=len(a))
bisect.bisect(a, x, lo=0, hi=len(a))
bisect.insort_left(a, x, lo=0, hi=len(a))
bisect.insort_right(a, x, lo=0, hi=len(a))
bisect.insort(a, x, lo=0, hi=len(a))
```





## Recursion

### Recursion with memory

```python
memo={}
def helper(i,j,..):
    if base_case(i,j): return
    if (i,j) not in memo:
        memo[(i,j)] =...
    return memo[(i,j)]
return helper(n,m)
```



## DP

```python
memo=[]
memo[0][0]=... #init base cases
for i ..:
    for j ..:
        memo[i][j]=... #transition
return memo[n][m]
```

### DP cases

### 1D, depend on $O(1)$ sub problem

![img](https://lh3.googleusercontent.com/43Zea3QY9VVzbIn71FtJbD_YxHEhh7fJJ8yKUis_1nLaEBkS-HR5xcScnP4Ahi-DwN_9VPNMGImphO4XDniFpiisDSOpFzQOuG71dynu-yHlIptyNQqbH4m4FiDWINem9itImyNGnQRRLYzzriZhngalAHv3u-o96H4e4oypO_p6i7gOw6PxuanbjkabHhM4PRruF8Yn23lzbRwRXVljrKzIxMqsM5lIc3sLvl6s7G5kPIBOq4K6DudtvvAD9yHn3OWWK1jgx5Yo6cp7mIzPOhDxAda7mdGurNd-r_S1V11k0TMCwfjRBswUormh2bpJjSZMp5OIwsEU6ZMdmk_ZtQND31V6WSugw_fvW3b1gW-1h9umyig2wxKrjbesXm_MvKD3jYDRuiYK2q_AgWeLdS3lgRAAxNSmBk6LOkJeabmBEC-30ZhA24LLoKLuOBCtLV9-GpMwEcZ0o57vA46yhDneEs4Oby20E1UAh6QaGatVRR5QyAkWn7JPzvtVB34Twr7T0YbC6qh0gB5xvTP-yMPjViAUXyT4CRVXC_JpkudCXFdGnHE0BV6ovxNrkYkEtQeItF8Uflu9XuGreNNWHuwShvwMV7G7wT8UF1HX07gonVHkTG5dugfcSHdCjRYB3jqqj1zvjPm8A-TIqk4pgV-tJDSXjQ9kxZgFF-orqnUMaa7slQZk9I1XPHrG=w1068-h802-no?authuser=0)

### 1D, depend on $O(n)$ subproblem

![img](https://lh3.googleusercontent.com/6oUrB9T1_flXYOb1OPCi19SLIJCLPvnFDi6Y4xxPFZs5M9HixOns1BQggvBk5d92zHA4AABgymlJpRQvkC49PLWv9VfEQJfmWXmHvFf_mLvLuOYGVY5fZP5PotCh6bnfWa2ufDpBo0jurQljypX8G3bif906BP8XfzEsu92Ev3y_lv_P2IQ__fZZXVD5z0SY6Qamrb80BtqMlptN0HG1PS9xaeATqQ1jjDF_4F-9B25g3O-DgrEZbsZaNayTA1VfB9x-fpVbhpL_ALo-L7BrwklUaIXa9hIvQLRe0GMbB2odianzpxKXwR5u1AWglq4E95BgnoioZS8gseyl5NZJoSbWPtAcS6M9nnSuy2uYMMw1GUvm-RpbG12GySPLvJeTkDd4Bz_cLBwBg8jhqux0Bwjs_KIotx60kdj8-5A-F9Zu3ahZKeWMwN4kKcTvjPBN4_-AeRgVz2B5_M-_-Pj8eYMQNeBN8xL7LppjsWHbkV5536ErRvUsxC-r1Ctck5JB5FoVk67TwKuoIZF5fnZJfqijh-jRAj8PQd6R4PFt796FbmKRTHfq4FlSByicqE2sVAGx8lfCw0KSH4ziFy1tUN7fF7CzmTMg44um_flPif8aUhxMieznXI8EsJusLiDX6MmT3vPrR9g23IiR1VgWkYA3ES50uOguA48pv7UjjiOEGBFVQ_sKzj8DstE8=w1068-h802-no?authuser=0)

![img](https://lh3.googleusercontent.com/earWR1Hb9E2qnWIaugEi68LpmajQPWIogPTihKGOr55QqO3RiqYZZHCQ_72Q9eGFkOvAswBpjMML55-5v7hHPlGTBf7yfOg10ZajMd6jIR6KoJLEyA5fD8MJcpkU_sEr49T9RbT-356nqYndHP3RaTBpDMenMPihOmzHdyKMirfRzRVq05Dpb1HHI2Z6gvUE1Gu3taiuh6PEdH02XfA8KLJK7cQ4FP-Vs9F1OcogP4HpRK-WRhf0UcOKjjtrK8NfMc1qeX1j7GfUxL_MWG-d__DyEIxt0jYedWiYO7-ziUqeZn9J-yo9t4j3vsYNrFDqpXGa0aUSWR9N7ZykCxeZMElUlofK2Nq5nkUqaJD3tyiNPQwLMb6CxSDaxgseT8fRLPYGbop7xNuRDW01GpC5NIlI4yPeTPenqdIkSON7CKLfevADuPiu548cWXsW_nmaFL0npctOWZ1SFARiPFr6OoKJQklvH3ilzre12CGb_JswDE7FLoai7h2CKb6FgmCiEJGpKPFED0SNSxVqBE2XCSltVK0zwQUSBCgcWo64Rk8PgprmSbKNteOlDjaxu6YxJt6_n64DWJKCQz7RqBIsm16M30xEVWsPLb2f4nDHC1E1sdAC0WDgg4yYoscTvnvmvS-VE1RCKJ_Ml5JH8me3tFBLi99teUKOO6xQT1tPsMPcKbxL2s0gWz5743DO=w1068-h802-no?authuser=0)

#### 1D, 2 sets of sub-problems

845 Longest Mountain in Array

```python
for i in range(len(A)):
    if A[i]>A[i-1] inc[i]=inc[i-1]+1
for i in reversed(range(len(A))):
    if A[i]<A[i-1] dec[i]=dec[i-1]+1 
ans=0
for i in range(len(A)):
    if inc[i] and dec[i]: 
        ans=max(ans, inc[i]+dec[i]+1)
```

926 Flip String to Monotone Increasing [TODO]

```python
for i in range(n):
    l[i]=l[i-1]+S[i]-'0'
for i in reversed(range(n-1)):
    r[i]=r[i+1]+'1'-S[i]
ans=r[0]
for i in range(n):
    ans=min(ans, l[i-1]+r[i])
```

#### 1D, multiple states

![img](https://lh3.googleusercontent.com/t8-DlGALJk2qmD9S1o71K-pKJ-WtQY8bMoVTeoLKhv05JVBnaSn_qGj5QKXxcJgyLQwnJnc0PxKuAtFyhNje2NfAaeIHypM0j0VN1Ml8cZwsiKqb1snkFl4jmlyPmD5P9-5g5Apa7x6jSo-2TwHhlA9Fyeo3WTXTEPDz9gl3Yh-GHj0EQ6d4GVr6-ZWufacZec9FdeIke6_TigMg60qukiclrP0Qweh7WyBHJGN_CzHduXArMdI9ERXLA07o021AMrY02H5RoCreWSTshltV3Joa3Q9K_G-uoe166ECqkbkADwI7WTYFvtJ7L7fKwFVsaSwdU-VyTDmmaJuAhCDUQo7M4Ezc8DOEmGcBziwZXh1MuvUMVZT0H2XJ4NsuBoTZKvowpNMVaJAtHFRza7DRsRQtfL-DHnNK770RbOtQ_6i4s8gPPdRXcjJ0AtQtm0gkY0zpTcLzW1Ev3vkON4hVv_wvvDqcruJvXTMCkWOyTxPOhkV_lVODm11-1qUf8c_WfAUTeyJgKTFpJ9eRsDNTqIBtuYybkLxT7gxZBS3nqSF1KRPu26HBCos45xK7RNFy6E4uDM4WEruhNJ_HQxdc_Cp1rV5t1Nh_dhIikxYYjmyOelmVMESWD0ZJ2eyjkWSxk8_GSUZk1YFmImql4-UDIFyEuFlXb-bGTMAjynfxG_o4Y6yKPhDCxqYFc4oB=w1068-h802-no?authuser=0)

### 2D

![img](https://lh3.googleusercontent.com/mY0fD3DUCLerrRC9R5K3XqHYEew-zcXcBE5axZtRWZ_23OyqxkjNzKFM0uEOx-H5E_n_6XBfPMHH37gVtW9TZgepdkPGoDEawEU8S0ezHIE3A6DmsCT1U6gj909Kf1ahQymnCUdP-OiQuAy3y2G6X0y_-gE71TFFtriHF4TM3UkNUntRc2oPXep0djz8peeX1g9dJVFBtLAcPOrN_At2A9lcNIfwv52IAViaIJiDcxO26bl_WOBBHhX6GwWOWUEkWYzbVkigN0a1G7udsgGHQ5U0-WrtPsDO2QCEgID6KUvjkTr-pByoCbAIBxc_rOHVzYaEaJgTUJMzJqjvlvHeXnCC2t2oT65QBxAlDUx-NEymJFbqEB3eoHfE0cG-L6qe8R9ra40gFeTAmp34aefpEdgcMesvVNfXiYPtOfgg4Vg_MLvJRrOdJdf9QGq4auhM4NU7ev7TV4OZVoWutiZltbEkFFK34CSJNTtxxhrHUyfsZcqAryox0hdBI7DXbuyYrWj64EY5yUYRzu2YVQOrlofo1dhyRQ8IFPGT60SWUJ3GQoeK0stASV95UuvrYyu20sGRuJP0kEqd-oHJh9kK4yR20vLm9qPAJrjMQd-whFqvhe8E6TuYm7G-vUhQclb1UHjtgQsDS5by6EjX1iVevd3JCdzkUlHnhKjM55sijn5V5n4UFeJt65GVemya=w1068-h802-no?authuser=0)

![img](https://lh3.googleusercontent.com/JSDxYM5P_MH055zMS1A59xsriiq7doOnS3rcyoDH91y95AAJxtrpBMdEUDC7IpNNW3Rs7xs8EH-hawzMN11vjeCh_FtQjcxV-sKqOKfm56FmduSn4XGbZ6TKJJVZ2K9tyHmy1DX9KrEnCbkywd7xaFNYkvAgMiVoljKAs3q1UYuaWr-uDuOLcIk0dpXL13ShBbLua2FM5LWIVGSyJbn0H6fOKVMrqbDAxh50y6X-ySus1mKkaLHtX6RM2jh-I-QP1mJjjj3fzeE0AL0i2mLpUkSgI35aU4dYl5cUqoHb_tysd_TyMjWS6VYLp2vhJ1GF3xOgzorwSGdup-mqgz5VMnNMrcucS3M3gjG9ZyOiv7iObqhsYfrfh7dr3SoBx72XcWj36cYAu3GhfA7wt5bDCNFUw9mtsg1FM1WPBYHPNJllQvOr8iq3BKI_U_s2wX_MtfxM1en0JFxQ2lnsvqCal4JlwmOING3pNsv3c0c-BXwxOrCt8EdwqZ1rgXWmcnJEL_j1lozcDwUvlwWETyss2yEqxkEIJejSE74V_Q0AvvWN_JKc4-jXSVzGlcaTAPR36ReMSSiEU1A9npLFS6WtD_hIQcvZBYqVjDsU6gaKvqlKpxCDxaFLyOAxZmMbcXqqvULgfmarOJXVGQwJ9gBtAwBWNRgoDw-mlTimlk_7e_LmwAAlk_MnlcBwMGyB=w1068-h802-no?authuser=0)

![img](https://lh3.googleusercontent.com/WPfaeEpIDElMluzwr37muFJ4SsJO4YIfWvnLd2xVwgayM_Id79PZ331b7aSO_6l9s0U4hO3YkWSuj549D1T7gjUXJ64_4s708mm10AnLvvIijgEeCA8fK-cxZTrjZxnai07J7MihZ_tuCH34-mlhIwsJibDY05wuf_jOmI67syZECTKdnVoW3OASOj0tJYIeNK2Qwg9jD98UJphp8k-2DcoftRzRYdjcPIIVDC-pL4Qwgpd0zJjffqQ5SBF9fFt7YlnEBmJbbkI0Zx0A6XxHheims4fIUEP1A4_GbgvyCVqI0D9X0rcLjaaXeTUE47VpCjFi9W7tILj0zBg7k__eHjoERYhbgc8SeHbuwZ7llXJ7qHFKI0-5yPEB2m0glya7kmb9rWc2O7hTAwitdKc0ekDoYv_KOmEP9UUWRDlzeYZmO2zSDTFqQXvIwPkYouIANT87RI0D9vhRTm6ig6BkPYs4Pis6hGf49hGvNrWFMXdhSXlu1e4RVvMxtoLwblBVUQry7FQgGr1-q4JLNiEuM1rCiBis2W-w66wkMPWl1EvxNP3DPbtkf2_Y5jxHwifas0RxT4etDfht5Cp2NWwWHtDbfthzO-Nkei3ckFJsdskZQOE--2QGS_DVopouEKlt1Sy4VE5V3-aMF-0dr0djiUJbJ3CI9ARk3q8mt1bAX8uCFEePWRxh5Id8otwR=w1068-h802-no?authuser=0)

![img](https://lh3.googleusercontent.com/zqDSmcl7dkDDSTHwUzt-O_vj_INOOMARb7x2t-eSQPsABAYqUDChC0nu6SshP4G6SR9Iue-Gv2Zz9kPzKRZ2r9BckrZ6MOK70YZMMjaVjcHmADHYQVitX45pB4mviA87KuhWvM0Nt2xocur-gcrg9ZtHWbRzWIJQ_ro4rrVFh2_KIpeJIIP8R5-7wsM-fVfuR77KhNk1Dp7_VV6kzONjMRceOmmB2WRe4zg-uXgeOshXiHfttOXnieXmHd82ijbfQWBcio6mrrop0SrAisQzBamhe_T0Yh2FMP7rnEnHEPi793N1RmvYtqE5-fDvsEjw_SRSzB3V5Mp0oW0Nz4ATpYbSGlJ4ogdri0n-fQs1r92f8ZaX1JQOgINbpJfvciGJqeCGYncMo5DQKOlclN0XOc1bdpy5Kj2MIHs8YUtHMPiVmQrSshma3YrV-TjjCKuV4Q4gfvJUjcgpdIF-7-8ilPvf0DHOb2xSqDeGhkKO3ECEQnSQUoZeawlqpm8JzafaxmP3GvZisIeKybZ56-e4-1nWudQv1Xhgi0Gbzn6_63Ke4BZ8RWWUlfSxmd1qLmARlMqQultUzh2QBe2CPpGImNXOpWw1fP5EhWDxyjuOrtXtpbreT-NCd8SPPFG2VBT_uGnoenJTrZqfs4k9IVLvM4N-roFmvEtyGLRh0mMHM2QhnelLV3oZylfW9TFd=w1068-h802-no?authuser=0)

## Search

### Binary Search

+ Time complexity: $O(log(r-1)*(f(m)+g(m)))$
+ returns left most element where g(m) holds

```python
def search(A, l, r):
     while l<r: # l<=r for [l,r]
            m=l+(r-l)//2 #to prevent overflow
            if f(m): return # early stopping critera
            if g(m): #interface critera
                r=m # r=m-1 for [l,r]
            else:
                l=m+1
     return if f(l).. # or not found
```

#### Examples - simple g(m)

+ Find bound [...)

  ```python
  def f(A, val, l, r): #l = 0, r = len(A)-1
      while l<r:
          m=l+(r-l)//2
          if A[m]>val: #find right most+1. A[m]>=val to find left most
              r=m
          else: l=m+1
      return l
  
  ```

+ 69  `floor(sqrt(x))`

  ```python
  def f(x):
      l=0
      r = x+1
      while l<r:
          m=l+(r-l)//2
          if m>x/m: #l**2>x
              r=m
          else:
              l=m+1
      return l-1
  ```

+ 278 First Bad Version

  ```python
  def f(x):
      l=0
      r=n
      while l<r:
          m=l+(r-l)//2
          if isBadVersion(m):
              r=m
          else: l=m+1
      return l
  ```

#### Examples - complex g(m)

[TODO]

![img](https://lh3.googleusercontent.com/Jj46SK33fgXW1K55p7HRtkhc3UQCRa9WnMGtaoda1A6WULM4Q5agIE0gMkKHcc7Ccj_ntxWhGbJ_14qbWA45ottfV9Sg7iCujSBnhSw_BEnOfxbvMLpLeOpl6e3HsSIsBGogVsQBjO9Xm2L31NOprTilpzzHh-vf7hQ-uMvhOTNakaC8oyEUd9PvmyBFfInEgT0pLTxSXlOEiskn1VSoRTXbAc-_-gqNWR3O8I5JnlpMoBbfkzj6zemrAwCQUadxjDs4c7x3bjg2f3Hnx6Y2rbHhDXvrDur1r4umU7nMXI1am-0McrMs5ol6kFFIRRRRIRbRGSqBsuPaCZn8pVkReTtjOTPKbfKYzEKZWFBWWu6paQtLTwOz6IxJ85hNxYQADmWev8R4eO-bt6PAXhxX9O7wv5SuwuRvGwLtiC9r_aij3x1-ypZ0Dx_JN2LuWSZqkhWeombhSpqwo6MH7Og6nKFymV46enJ1pw_uRZyafAjAwoganOVHQZxsunyaxTzIC5sa5Lre7xCkcRs_YQJIAUj6EVCox1E18_sabIpsGdDEDGrIvszjNSEjOf2Wz2GlT1nFVwgVU4lfsGA0phqfq1HJLYwPfyI57X6XBXn1zeFaaDQMKg-yTdiF4tyVEQZhfR-Cgp_KXR8JONjP68sHb3kooFRgeb6heId8lAT5CROQvh64ghpL1YDYqPy_=w1068-h802-no?authuser=0)





### DFS & BFS

+ grid:

```python
if not A return False
n,m = len(A),len(A[0])
ans;
for i in range(n):
    for j in range(m):
        ans = f(ans, DFS(i,j))
return ans

def DFS(i,j):
    if illegal(i,j): return False
    if earlyStop(grid[i][j]): return
    if base_case(i,j): return #base case, usually gives True
    cur = grid[i][j]
    grid[i][j]=None #prevent reuse
    res = LOGIC(DFS(d) for d in directions) #(i-1,j)(i+1,j)(i,j-1)(i,j+1)
    grid[i][j]=cur #put back if backtracking needed
    return res

def BFS(i,j):
    seen=()
    queue = deque((i,j))
    while queue:
        cur = queue.popleft()
        for d in directions:
            if illegal(d): 
                seen.append(d) #prevent reuse
                continue
            if earlyStop(d): return result
            seen.add(d) #prevent reuse
            queue.append(d)
    return NOT_FOUND

def illegal(i,j):
    if i<0 or j<0 or i>=n or j>=m: return True
    if (i,j) in seen: return True
    if notAvailable(i,j): return True #blocked grid
    return False

def earlyStop(i,j):
    if (i,j) = target: return True
```





#### Example

+ 79 word search in grid - DFS with backtracking

  ```python
  n = len(board)
  m = len(board[0])
  for i in range(n):
      for j in range(m):
          if DFS(board, word, 0, i,j) return True
  return False
  
  def DFS(board, word, d, i,j):
      if i<0 or i>= n or j<0 or j>=m: return False
      if word[d] != board[i][j] return False
  	if d==len(word)-1: return True
      cur = board[i][j]
      board[i][j]=0 #prevent reuse
      found = DFS(board, word,d+1, i+1,j) 
      		or DFS(board, word,d+1, i-1,j) 
          	or DFS(board, word,d+1, i,j+1) 
              or DFS(board, word,d+1, i,j-1)
      board[i][j]=cur
      return found    
  ```

+ 200 number of islands - no backtracking

  ```python
  if not grid: return 0
  m, n = len(grid), len(grid[0])
  ans = 0
  for i in range(n):
      for j in range(m):
          if grid[i][j]=='1':
              ans+=1
              dfs(grid,i,j,m,n)
  return ans
  
  def dfs(A,i,j,m,n):
      if illegal(A,i,j,m,n): return
      A[i][j]='0'
      dfs(A,i+1,j,m,n)
      dfs(A,i,j+1,m,n)
      dfs(A,i-1,j,m,n)
      dfs(A,i,j-1,m,n)    
      
  def illegal(A,i,j,m,n):
      if i<0 or j<0 or i>=m or j>=m: return True
      if A[i][j]=='0': return True
      return False
      
  ```

  



## Binary Tree

### One root

```python
def f(root):
    if not root: return
    if base_case(root): return #(eg. if not root.left and not root.right)
    l = f(root.left)
    r=f(root.right)
    return g(root, l, r)
```

#### Examples

+ 104 max depth

  ```python
  def maxDepth(root):
      if not root: return 0
      l = maxDepth(root.left)
      r = maxDepth(root.right)
      return max(l,r)+1
  ```

+ 111 min depth (root-leaf)

  ```python
  def minDepth(root):
      if not root: return 0
      if not root.left and not root.right: return 1 #leaf
      l = minDepth(root.left)
      r = minDepth(root.right)
      if not root.left: return 1+r
      if not root.right: return 1+l
      return min(l,r)+1
  ```

+ 112 Path Sum

  ```python
  def pathSum(root, sum):
      if not root: return false
      if not root.left and not root.right: return root.val==sum
      l = pathSum(root.left, sum-root.val)
      r = pathSum(root.right, sum-root.val)
      return l or r
  ```

### Two roots

```python
def f(p, q):
    if not p and not q: return
    if base_case(p,q): return #(eg: if not p or not q)
    l = f(p.child, q.child)
    r = f(p.child, q.child)
    return g(p,q,l,r)
```

#### Examples

+ 100 Same Tree

  ```python
  def f(p,q):
      if not p and not q: return True
      if not p or not q: return False
      l = f(p.left,q.left)
      r = f(p.right, q.right)
      return p.val==q.val and l and r
  ```

+ 101 Symmetric Tree

  ```python
  def f(p,q):
      if not p and not q: return True
      if not p or not q: return False
      l = f(p.left, q.right)
      r = f(p.right, q.left)
      return p.val==q.val and l and r
  ```

+ 951 Flip equivilant binary trees (can either flip or not flip)

  ```python
  def f(p,q):
      if not p and not q: return True
      if not p or not q: return False
      l1 = f(p.left, q.left)
      l2 = f(p.left, q.right)
      r1 = f(p.right, q.right)
      r2 = f(p.right, q.left)
      return p.val==q.val and ((l1 and r1) or (l2 and r2))
      
  ```

  

