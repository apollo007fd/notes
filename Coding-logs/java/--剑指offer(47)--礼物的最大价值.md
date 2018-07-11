在一个m*n的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向左或者向下移动一格，知道到达棋盘的右下角。给定一个棋盘及其上面的礼物，请计算你最多能拿多少价值的礼物？

# 解法一 递归法  自上而下
定义函数f(int[][] board, int m, int n)求从（m,n）开始到最右下角可以得到的最大礼物价值，  
当m+1>=board.length或n+1>=board[0].length的时候，f=0，  
那么有:  
f(board, m, n) = board[m][n] + max(f(board, m+1, n), f(board, m, n+1))  

    public class Bonus {
        public int getMost(int[][] board) {
            if(board==null)
                return 0;
            return helper(board, 0, 0);
        }

        public int helper(int[][] board, int i, int j){
            if(i==board.length-1 && j==board[0].length)
                return board[i][j];
            int down=0, right=0;
            if(i+1 < board.length)
                down = helper(board, i+1, j);
            if(j+1 < board[0].length)
                right = helper(board, i, j+1);
            return board[i][j] + Math.max(down, right);
        }
    }

# 解法二 动态规划  自下而上
由于解法一中存在很多重复求解个过程，所以可以用动态规划求解，
从右下角开始，先往左，再往上，逐渐求各个位置到右下角的最大价值；

        public class Bonus {
            public int getMost(int[][] board) {
                int height = board.length, width = board[0].length;
                int[][] most = new int[height][width];  //most数组存放当前位置到最右下角的最大价值
                int min = Math.min(height, width);
                for(int i=1; i<=min; i++){
                    int down, right;
                    int row, col;
                    // 从右下角开始更新most数组，分别往左进行横排更新，往上进行竖排更新
                    // 往左横排更新most
                    row = height - i;
                    for(col=width-i; col>=0; col--){
                        down = row+1 >= height ? 0 : most[row+1][col];
                        right = col+1 >= width ? 0 : most[row][col+1];
                        most[row][col] = board[row][col] + Math.max(down, right);
                    }
                    // 往上竖排更新most
                    col = width - i;
                    for(row=height-1-i; row>=0; row--){   // 注意伪对角线上的元素不能重复更新
                        down = row+1 >= height ? 0 : most[row+1][col];
                        right = col+1 >= width ? 0 : most[row][col+1];
                        most[row][col] = board[row][col] + Math.max(down, right);
                    }
                }

                return most[0][0];
            }
        }

# 解法三 动态规划更省时间的解法

        public class Bonus {
            //基于动态规划的思想，不仅仅局限于6*6矩阵，适用于所有的N*M矩阵以及所有的方阵。
            public int getMost(int[][] board) { 
                //两个for循环用来遍历二维数组不用多说。
                for(int i = 0 ; i<board.length ; i++){
                    for(int j = 0 ; j <board[0].length ; j++){
                        if(i==0&&j==0){
                            //如果是起点坐标，不做任何处理。
                        }else if(i == 0){
                            //如果走在行的临界边，也就是第一行，那么他只能向右走
                            //向右走的时候该点就要将后面的值加起来。
                            board[i][j] += board[i][j-1];
                        }else if(j == 0){
                            //如果走在列的临界边，也就是第一列，那么他只能向下走
                            //向下走的时候该点就要将上面的值加起来。
                            board[i][j] += board[i-1][j];
                        }else{
                            //核心点在这，除去两个临界边，剩下的就是既能向右走，也能向下走，
                            //那么这时候就要考虑走到当前点的所有可能得情况，也就是走到当前点
                            //各自路径的和是不是这些所有到达该点路径当中最大的了。
                            //temup用来存储从该点上面走下来的最大路径和。
                            //templeft用来存储从该点左边走过来的最大路径的和，
                            int temup = board[i-1][j];
                            int templeft = board[i][j-1];
                          //这两者肯定只能选其一，进行比较，那个大，就把这个值加给当前点，
                          //因为从一开始我们就进行了大小的比较，每一个点存储的都是到达当前点
                          //的最大值。所以直到最后一个点为止，她的值就是当前最大值的和。只要返回
                          //最后一个点的内容就可以了。
                            if(temup>templeft){
                                board[i][j] +=temup ;
                            }else{
                                board[i][j] +=templeft;
                            }
                        }
                    }
                }
                /*  初始数组的情况。
                    564 448 654 186 490 699
                    487 444 563 228 365 261
                    429 505 612 564 715 726
                    464 617 234 647 702 263
                    245 249 231 462 453 646
                    669 510 492 512 622 135 
                 */
                /*结束后返回的数组。
                564     1012    1666    1852    2342    3041   
                1051    1495    2229    2457    2822    3302   
                1480    2000    2841    3405    4120    4846   
                1944    2617    3075    4052    4822    5109   
                2189    2866    3306    4514    5275    5921   
                2858    3376    3868    5026    5897    6056
                可以看到，最后一个坐标点的值6056，他就是当前最优的路径所得出来的值
                 */
                return  board[board.length-1][board[0].length-1];
            }
        }
        
# 空间复杂度为o(n)的动态规划解法  
见书本
