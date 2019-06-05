


### 1. largest common factor
ref: 1071. Greatest Common Divisor of Strings
- if a or b is zero, we return non-zero elements, because everything divides zero
```
public int gcd(int a, int b) { 
    // Everything divides 0  
    if (a == 0) 
      return b; 
    if (b == 0) 
      return a; 

    // base case 
    if (a == b) 
        return a; 

    // a is greater 
    if (a > b) 
        return gcd(a-b, b); 
    return gcd(a, b-a); 
 } 
```
- oneline solution:
```
public int gcd(int a, int b) {
    return b > 0?gcd(b, a % b):a;
}
```


### 2. rectangle area sum
- give a set of points, check if we can form rectangle/find max area/min area
- key idea is to use hash map to store elements with same x/y, and then O(n^2) defining diagonal points of the rectangle, update return value
- practise: 939
```
public int minAreaRect(int[][] points) {
        Map<Integer, Set<Integer>> map = new HashMap(); //x -> y's
        for(int[] p: points) {
            map.putIfAbsent(p[0], new HashSet());
            map.get(p[0]).add(p[1]);
        }
        int ret = Integer.MAX_VALUE;
        for(int[] i: points) {
            for(int[] j: points) {
                if(i[0] == j[0] || i[1] == j[1]) continue;
                if(!map.get(i[0]).contains(j[1]) || !map.get(j[0]).contains(i[1])) continue;
                ret = Math.min(ret, Math.abs(i[0]-j[0])*Math.abs(i[1]-j[1]));
            }
        }
        return ret == Integer.MAX_VALUE?0:ret;
    }
```

### 3. rectangle idea: scaling
#### 3.1 scale up
- some times we can think about signifying the properties of the each rectangle to simplify our coding purpose
- practise lc959:
    - In a N x N grid composed of 1 x 1 squares, each 1 x 1 square consists of a /, \, or blank space.  These characters divide the square into contiguous regions.
(Note that backslash characters are escaped, so a \ is represented as "\\".)
Return the number of regions.
    - we can think about enlarge previous 1x1 sub rectangle into 3x3. The reason 2x2 is not applicable is that we need to take care of diagonal cases when splitting as 2x2.
    - after scaling up, the question become pure union find/dfs
```
class Solution {
    int[][] direct = new int[][]{{1,0},{-1,0},{0,1},{0,-1}};
    public void dfs(int[][] grid, int i, int j) {
        grid[i][j] = 1;
        for(int k = 0;k < 4;k++) {
            int r = direct[k][0]+i;
            int c = direct[k][1]+j;
            if(r<0||r>=grid.length||c<0||c>=grid.length||grid[r][c]==1) continue;
            dfs(grid, r, c);
        }
    }
    public int regionsBySlashes(String[] s) {
        int n = s.length, ret = 0;
        int[][] grid = new int[n*3][n*3];
        for(int i = 0;i < s.length;i++) {
            for(int j = 0;j < s[0].length();j++) {
                char cur = s[i].charAt(j);
                if(cur == ' ') continue;
                if(cur == '/') {
                    grid[i*3][j*3+2] = 1;
                    grid[i*3+1][j*3+1] = 1;
                    grid[i*3+2][j*3] = 1;
                }else{
                    grid[i*3][j*3] = 1;
                    grid[i*3+1][j*3+1] = 1;
                    grid[i*3+2][j*3+2] = 1;
                }
            }
        }
        for(int i = 0;i < 3*n;i++) {
            for(int j = 0;j < 3*n;j++) {
                if(grid[i][j] == 1) continue;
                ret++;
                dfs(grid, i, j);
            }
        }
        return ret;
    }
}
```

### 4. calculate triangle area by three vertex
- LC 812
```
    public double largestTriangleArea(int[][] points) {
        double ret = 0;
        for(int i = 0;i < points.length;i++) {
            for(int j = i+1;j < points.length;j++) {
                for(int k = j+1;k < points.length;k++) {
                    int[] a = points[i], b = points[j], c = points[k];
                    double area = 0.5*Math.abs(a[0]*b[1]+b[0]*c[1]+c[0]*a[1]-b[0]*a[1] - b[1]*c[0] - c[1]*a[0]);
                    ret = Math.max(ret, area);
                }
            }
        }
        return ret;
    }
```
