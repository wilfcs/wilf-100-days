# [1539. Kth Missing Positive Number](https://leetcode.com/problems/kth-missing-positive-number/description/)

## [Approaches](https://takeuforward.org/arrays/kth-missing-positive-number/)

## Codes
1. Brute Force:
```cpp
class Solution {
public:
    int findKthPositive(vector<int>& arr, int k) {
        for(int i=0; i<arr.size(); i++){
            if(k<arr[i]) return k;
            k++;
        }
        return k;
    }
};
```

2. Binary Search
```cpp
class Solution {
public:
    // Function to find the kth missing positive integer in a strictly increasing array
    int findKthPositive(vector<int>& arr, int k) {
        // Get the size of the input array
        int n = arr.size();

        // Initialize low and high pointers for binary search
        int low = 0, high = n - 1;

        // Binary search to find the position where the kth missing positive integer lies
        while (low <= high) {
            // Calculate the middle index
            int mid = (low + high) / 2;

            // Calculate the number of missing positive integers before the middle element
            int missing = arr[mid] - (mid + 1);

            // If the number of missing positive integers before mid is less than k
            if (missing < k) {
                // Update low to search in the right half of the array
                low = mid + 1;
            }
            else {
                // Update high to search in the left half of the array
                high = mid - 1;
            }
        }

        // Return the kth missing positive integer
        // k + high + 1 gives the actual missing positive integer in the array
        return k + high + 1;
    }
};
```

### Max-min/min-max problems start here

# [ Aggressive Cows](https://www.codingninjas.com/studio/problems/aggressive-cows_1082559?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf)
### Min-max problem

## [Approach](https://takeuforward.org/data-structure/aggressive-cows-detailed-solution/)

## Code ->
```cpp
int numCows(vector<int> &stalls, int mid){
    int cows = 1, last = stalls[0];

    for(int i=1; i<stalls.size(); i++){
        if(stalls[i] - last >= mid){
            cows++;
            last = stalls[i];
        }
    }
    return cows;
}

int aggressiveCows(vector<int> &stalls, int k)
{
    //    Write your code here.
    int low = 1;
    int high = accumulate(stalls.begin(), stalls.end(), 0);
    int mid;

    sort(stalls.begin(), stalls.end());

    while(low<=high){
        mid = low + (high-low)/2;

        int cows = numCows(stalls, mid);

        if(cows>=k) low = mid+1;
        else high = mid-1;
    }
    return high;
}
```

# [ Allocate Books](https://www.codingninjas.com/studio/problems/allocate-books_1090540?utm_source=youtube&utm_medium=affiliate&utm_campaign=codestudio_Striver_BinarySeries&leftPanelTabValue=PROBLEM)
### Max-min problem. See the brute force becausea its way easier.

## [Approaches](https://takeuforward.org/data-structure/allocate-minimum-number-of-pages/)

## Code ->
```cpp
// Function to count the number of students needed for a given maximum number of pages
int countStudents(vector<int> &arr, int pages) {
    int n = arr.size(); // size of array.
    int students = 1;
    long long pagesStudent = 0;

    // Iterate through the array of pages
    for (int i = 0; i < n; i++) {
        if (pagesStudent + arr[i] <= pages) {
            // Add pages to the current student
            pagesStudent += arr[i];
        }
        else {
            // Add pages to the next student
            students++;
            pagesStudent = arr[i];
        }
    }
    return students;
}

int findPages(vector<int>& arr, int n, int m) {
    //book allocation impossible:
    if (m > n) return -1;

    int low = *max_element(arr.begin(), arr.end()); // low will always be the max element because we can't make a book with anything less
    int high = accumulate(arr.begin(), arr.end(), 0);

    for (int pages = low; pages <= high; pages++) {
        if (countStudents(arr, pages) == m) {
            return pages;
        }
    }
    return low;
}
```

## Code ->
```cpp
// Function to count the number of students needed for a given maximum number of pages
int countStudents(vector<int> &arr, int pages) {
    int n = arr.size(); // size of array.
    int students = 1;
    long long pagesStudent = 0;

    // Iterate through the array of pages
    for (int i = 0; i < n; i++) {
        if (pagesStudent + arr[i] <= pages) {
            // Add pages to the current student
            pagesStudent += arr[i];
        }
        else {
            // Add pages to the next student
            students++;
            pagesStudent = arr[i];
        }
    }
    return students;
}

// Function to find the minimum number of pages such that the maximum pages assigned to a student is minimum
int findPages(vector<int>& arr, int n, int m) {
    // Book allocation is impossible if the number of students is greater than the number of books
    if (m > n) return -1;

    int low = *max_element(arr.begin(), arr.end()); // Initialize the minimum pages to the maximum pages in a single book
    int high = accumulate(arr.begin(), arr.end(), 0); // Initialize the maximum pages to the sum of all pages in all books

    // Perform binary search to find the minimum number of pages
    while (low <= high) {
        int mid = (low + high) / 2; // Calculate the middle value
        int students = countStudents(arr, mid); // Get the count of students needed for the current maximum number of pages

        if (students > m) {
            low = mid + 1; // If more students are needed, increase the minimum pages
        }
        else {
            high = mid - 1; // If the current number of students is sufficient, decrease the maximum pages
        }
    }
    return low; // Return the minimum number of pages
}
```

### watch vid if you still don't understand

# [410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/description/)

## [Approaches](https://takeuforward.org/arrays/split-array-largest-sum/)

## Codes ->
1. Brute Force
```cpp
class Solution {
public:
    int numOfSubarrays(vector<int> &nums, int check){
        int sub = 1, sum = 0;

        for(int i=0; i<nums.size(); i++){
            if(sum + nums[i] > check){
                sub++;
                sum = nums[i];
            }
            else{
                sum += nums[i];
            }
        }
        return sub;
    }

    int splitArray(vector<int>& nums, int k) {
        int low = *max_element(nums.begin(), nums.end());
        int high = accumulate(nums.begin(), nums.end(), 0);
        int mid;

        for(int i=low; i<=high; i++){
            int sub = numOfSubarrays(nums, i);
            cout << sub << " " << i << endl;
            if(sub == k) return i;
        }
        return low; 
    }
};
```
2. BS
```cpp
class Solution {
public:
    int numOfSubarrays(vector<int> &nums, int check){
        int sub = 1, sum = 0;

        for(int i=0; i<nums.size(); i++){
            if(sum + nums[i] > check){
                sub++;
                sum = nums[i];
            }
            else{
                sum += nums[i];
            }
        }
        return sub;
    }

    int splitArray(vector<int>& nums, int k) {
        int low = *max_element(nums.begin(), nums.end());
        int high = accumulate(nums.begin(), nums.end(), 0);
        int mid;

        while(low <= high){
            mid = low + (high-low)/2;

            int sub = numOfSubarrays(nums, mid);

            // note that the condition over here is sub>k and not sub>=k beacuse we are trying to find min
            if(sub>k) low = mid+1; 
            else high = mid - 1;
        }
        // note that we are returning low and not high. 
        return low;
    }
};
```
# [Painter's Partition Problem](https://www.codingninjas.com/studio/problems/painter-s-partition-problem_1089557?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf)

## Approach 
Same as of last two questions

# [Row with max 1s](https://www.codingninjas.com/studio/problems/row-of-a-matrix-with-maximum-ones_982768?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf)

## Code ->
```cpp
int lowerBound(vector<int> arr, int n, int x) {
    int low = 0, high = n - 1;
    int ans = n;

    while (low <= high) {
        int mid = (low + high) / 2;
        // maybe an answer
        if (arr[mid] >= x) {
            ans = mid;
            //look for smaller index on the left
            high = mid - 1;
        }
        else {
            low = mid + 1; // look on the right
        }
    }
    return ans;
}
int rowWithMax1s(vector<vector<int>> &matrix, int n, int m) {
    int cnt_max = 0;
    int index = -1;

    //traverse the rows:
    for (int i = 0; i < n; i++) {
        // get the number of 1's:
        int cnt_ones = m - lowerBound(matrix[i], m, 1);
        if (cnt_ones > cnt_max) {
            cnt_max = cnt_ones;
            index = i;
        }
    }
    return index;
}
```

# [74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/description/)

## Approaches ->
1. Brute Force - Traverse in O(n*m) TC and find the target.
2. Better Approach: Place yourself on either the top right or bottom left cornor, that way when you go to your left the elements decrease and when you go down the elements increase. Using this you can find the target in O(n+m) TC.
3. Most Optimal: Treat 2D array as 1D array and perform Binary Search. TC -> O(log(n*m))

## Codes->
2.
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int row = 0, col = matrix[0].size()-1;

        while(row<matrix.size() && col>=0){
            if(matrix[row][col]==target) return true;
            else if(matrix[row][col]>target) col--;
            else row++;
        }
        return false;
    }
};
```
3. 
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix[0].size(); int n = matrix.size();

        int low = 0, high = (m*n)-1, mid;

        while(low<=high){
            mid = (low+high)/2;

            // Treat 2d as 1d using this formula
            int val = matrix[mid/m][mid%m];

            if(val==target) return true;
            if(val>target) high = mid-1;
            else low = mid+1;
        }
        return false;
    }
};
```

# [240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/description/)

## Approach ->
Literally all the approaches of the question above works.

# [1901. Find a Peak Element II](https://leetcode.com/problems/find-a-peak-element-ii/description/)

## Approach ->
A little tweak in find peak element 1, go revise the hill concept of that q and then come here.
The approach involves mimicking the experience of traversing a mountain range, column by column. For each column, we identify the highest point. Then, we see the immediate left and right from this peak to determine if it's the peak element (no need to check up and down because it is the greatest element we've picked in the column). If a higher point exists to the right, we eliminate the left half of the search space; if it's on the left, we eliminate the right half. This process continues until we find the peak or exhaust the search space. 

## Code ->
```cpp
class Solution {
public:
    // Helper function to find the row with the greatest value in a specific column
    int findGreatest(vector<vector<int>> &mat, int col) {
        int maxi = mat[0][col], row = 0;
        for (int i = 1; i < mat.size(); i++) {
            if (mat[i][col] > maxi) {
                maxi = mat[i][col];
                row = i;
            }
        }
        return row;
    }

    // Main function to find a peak element in a 2D grid
    vector<int> findPeakGrid(vector<vector<int>> &mat) {
        int row = mat.size() - 1, col = mat[0].size() - 1;
        int low = 0, high = col, mid;

        // Binary search for the peak element
        while (low <= high) {
            mid = low + (high - low) / 2;

            // Find the row with the greatest value in the current column
            row = findGreatest(mat, mid);

            // Determine values to the left and right of the current position
            int left = (mid - 1 >= 0) ? mat[row][mid - 1] : -1;
            int right = (mid + 1 <= col) ? mat[row][mid + 1] : -1;
            int val = mat[row][mid];

            // Check if the current position is a peak element
            if (val > left && val > right)
                return {row, mid};
            else if (val < left)
                high = mid - 1;
            else
                low = mid + 1;
        }
        // If no peak element is found, return {-1, -1}
        return {-1, -1};
    }
};
```