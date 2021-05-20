# Job-Scheduling



##### greedy algorithm

```java
static int greedy(int n,int[] t,int m){
        int[] L=new int[m+1]; //L[i]는 i번째 기계가 총 수행한 시간
        for(int i=1;i<=m;i++)
            L[i]=0; // 처음에 0으로 초기화
        int j=0;
        int count=t.length;
        while(count>0){
            for(int k=2;k<=m;k++) {
                if (L[1]<=L[k]){ 
                    L[1]+=t[j]; 
                    j++;
                }
                else{
                    L[k]+=t[j];
                    j++;
                }
            }
            count--;
        }
        Arrays.sort(L);
        return L[m];
    }
```

위와 같이 그리디 알고리즘은 먼저 끝난 기계에 작업을 할당하여 더 나중에 끝나는 기계가 수행한 시간이 총 걸린 시간이 된다.

while문 안에 for문에서 m=2이기 때문에 기계 1이 기계 2보다 먼저 끝나거나 동시에 끝나면 다음 작업은 기계 1이 하고 그렇지 않으면 기계 2가 한다. 작업이 끝나고나서 수행시간이 더 오래 걸린 기계의 시간을 반환한다.

##### bruteforce algorithm

```java
 static int bruteForce(int n,int[] t,int m){
        int[] L=new int[m+1];  //L[i]는 i번째 기계가 총 수행한 시간
        int[] minTime=new int[n];

        for(int i=1;i<=m;i++)
            L[i]=0;  // 처음에 0으로 초기화
        int j;
        for(int i=0;i<n;i++){
            j=i;
            L[1]=L[2]=0;
            while(j>0) {
                for(int k=0;k<n;k++) {
                    L[1] += t[j];
                    j--;
                }
            }
            while(j<n) {
                L[2] += t[j];
                j++;
            }
            if(L[1]>L[2])
                minTime[i]=L[1];
            else
                minTime[i]=L[2];
        }
        Arrays.sort(minTime);
        return minTime[1];
    }
```

m=2, 즉 기계가 2개이므로 while문에서 j값으로 기계에 작업을 몇개로 나눌지 설정한다.

나눠진 작업수대로 각각의 수행시간에 더한다.



위와 같이 brute force 알고리즘도 작성해 보았는데,만약 n이 4라면, 기계 1과 기계 2에 각각 0개, 4개부터 4개, 0개까지 분할하는 것까진 성공하였지만, 기계 1과 기계 2에 각각 1개, 3개로 분할하는 경우 기계 1에는 첫 번째 작업만 들어가고, 나머지의 경우로는 분할하지 못하였습니다.  그래서 밑에 손코딩으로 작성하였습니다.

greedy force 알고리즘을 손코딩 할 때, 기계는 앞에서부터(즉, 기계 1) 들어가게 했고, 한 쪽 기계에 아무것도 안 들어가고 나머지 기계에 전부터 들어가는 경우는 해가 될 수 없으므로 손 코딩에서 제외했습니다.
#### n이 2일 때

작업시간 ={2,5}

##### brute force

| 기계1 | 기계2 | 총 걸리는 시간 |
| ----- | ----- | -------------- |
| 2     | 5     | 5              |
| 5     | 2     | 5              |

->총 걸리는 시간 5초

##### greedy Algorithm

기계1: 2초

기계2 : 5초

->총 걸리는 시간 5초

근사 비율 : brute force/greedy=1



#### n이 3일 때

작업시간 ={3,5,8}

##### brute force

| 기계 1 | 기계 2 | 총 걸리는 시간 |
| ------ | :----- | -------------- |
| 3      | 5,8    | 13             |
| 5      | 3,8    | 11             |
| 8      | 3,5    | 8              |
| 3,5    | 8      | 8              |
| 3,8    | 5      | 11             |
| 5,8    | 3      | 13             |

-> 8초

##### greedy Algorithm

기계 1 :  3초+8초

기계 2 :  5초

총 걸리는 시간 : 11초

근사 비율 : brute force/greedy= 0.73



#### n이 4일 때

작업시간 ={2,4,7,8};

##### brute force

| 기계 1 | 기계 2 | 총 걸리는 시간 |
| ------ | ------ | -------------- |
| 2      | 4,7,8  | 19             |
| 4      | 2,7,8  | 17             |
| 7      | 2,4,8  | 14             |
| 8      | 2,4,7  | 13             |
| 2,4    | 7,8    | 15             |
| 2,7    | 4,8    | 12             |
| 2,8    | 4,7    | 11             |
| 4,7    | 2,8    | 11             |
| 4,8    | 2,7    | 12             |
| 7,8    | 2,4    | 15             |
| 2,4,7  | 8      | 13             |
| 2,4,8  | 7      | 14             |
| 2,7,8  | 4      | 17             |
| 4,7,8  | 2      | 19             |

->11초

##### greedy Algorithm

기계 1 :  2초+7초

기계 2 :  4초+8초

총 걸리는 시간 : 12초

근사 비율 : brute force/greedy= 0.92



#### n이 5일 때

작업시간 ={1,3,4,8,9};

##### brute force

| 기계 1  | 기계 2  | 총 걸리는 시간 |
| ------- | ------- | -------------- |
| 1       | 3,4,8,9 | 24             |
| 3       | 1,4,8,9 | 22             |
| 4       | 1,3,8,9 | 21             |
| 8       | 1,3,4,9 | 17             |
| 9       | 1,3,4,8 | 16             |
| 1,3     | 4,8,9   | 21             |
| 1,4     | 3,8,9   | 20             |
| 1,8     | 3,4,9   | 16             |
| 1,9     | 3,4,8   | 15             |
| 3,4     | 1,8,9   | 18             |
| 3,8     | 1,4,9   | 14             |
| 3,9     | 1,4,8   | 13             |
| 4,8     | 1,3,9   | 13             |
| 4,9     | 1,3,8   | 13             |
| 8,9     | 1,3,4   | 17             |
| 1,3,4   | 8,9     | 17             |
| 1,3,8   | 4,9     | 13             |
| 1,3,9   | 4,8     | 13             |
| 1,4,8   | 3,9     | 13             |
| 1,4,9   | 3,8     | 14             |
| 1,8,9   | 3,4     | 18             |
| 3,4,8   | 1,9     | 15             |
| 3,4,9   | 1,8     | 16             |
| 3,8,9   | 1,4     | 20             |
| 4,8,9   | 1,3     | 21             |
| 1,3,4,8 | 9       | 16             |
| 1,3,4,9 | 8       | 17             |
| 1,3,8,9 | 4       | 21             |
| 1,4,8,9 | 3       | 22             |
| 3,4,8,9 | 1       | 24             |

->13초

##### greedy Algorithm

기계 1 :  1초+4초+9초

기계 2 :  3초+8초

총 걸리는 시간 : 14초

근사 비율 : brute force/greedy= 0.93



|          | n=2  | n=3  | n=4  | n=5  |
| -------- | ---- | ---- | ---- | ---- |
| 근사비율 | 1    | 0.73 | 0.92 | 0.93 |

이와 같이 근사비율로 볼때 brute force알고리즘은 조금이지만 대부분 최적해가 아니라는 것을 알 수 있다.
