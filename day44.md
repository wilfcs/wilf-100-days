
# [205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/description/)

## Approach ->
The provided C++ code determines whether two given strings, s and t, are isomorphic. It uses an unordered_map to store the mapping of characters from s to t and a set to track characters in t that have been used as a mapping. The code iterates through the characters, ensuring that no two characters in s map to the same character in t, and checks for isomorphism conditions. If the lengths of the strings are different or the mapping conditions are violated, the function returns false; otherwise, it returns true.

## Code->
```cpp
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        // Using an unordered_map to store the mapping of characters from string s to string t
        unordered_map<char, char> mp;
        // Using a set to keep track of characters in string t that have already been mapped
        set<char> st;
        
        // Check if the lengths of both strings are not equal, return false if they are not
        if(s.size()!=t.size()) return false;

        // Iterate through each character in the strings
        for(int i=0; i<s.size(); i++){
            // Check if the current character in string s has already been mapped
            if(mp.count(s[i])){
                // If mapped, check if the mapping is the same as the corresponding character in string t
                if(mp[s[i]]!=t[i]) return false;
            }
            else{
                // If the character in string s is not mapped yet
                // Check if the corresponding character in string t has already been used as a mapping
                if(st.find(t[i])!=st.end()) return false;
                
                // If not, create a mapping from the character in string s to the character in string t
                mp[s[i]] = t[i];
                // Add the character in string t to the set to mark it as used
                st.insert(t[i]);
            }
        }
        
        // If all characters in string s can be mapped to string t, return true
        return true;
    }
};
```

# [796. Rotate String](https://leetcode.com/problems/rotate-string/description/)

## Approach ->
We can use the typical approach of rotating the string and then finding if the two strings match but we will have to rotate the string from every position to avoid edge cases. Instead of that complex approach just use this one liner..

## Code ->
```cpp
class Solution {
public:
    bool rotateString(string s, string goal) {
        return s.size() == goal.size() && (s + s).find(goal) != string::npos;
    }
};
```

## Explanation ->
s.size() == goal.size(): This condition checks if the lengths of the two strings s and goal are equal. If they are not of the same length, it's impossible for s to become goal through rotations.

(s + s).find(goal) != string::npos: This part checks if the concatenated string (s + s) contains the string goal. The find function returns the position of the first occurrence of goal in the concatenated string. If goal is not found, find returns string::npos, which is a special constant representing "no position" or "not found."

If find does not return npos, it means that goal is found in the concatenated string, indicating that s can become goal after some rotations.

If find returns npos, it means that goal is not found in the concatenated string, and therefore, s cannot become goal through rotations.

TC -> O(len(s) * len(goal)).

## Alternative Code 
```cpp
class Solution {
public:
    bool rotateString(string s, string goal) {
        for(int i=0;i<=s.size();i++){
            s.push_back(s[0]);
            string temp=s.erase(0,1);
            if(s==goal){
                return true;
            }
        }
        return false;
        
    }
};
```

TC -> Same as above

# [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/description/)

## Approaches ->
1. Sort the strings and compare. TC-> O(n log n)
2. Store them in hashmap and compare. TC-> O(n). SC-> O(1) because we only have 26 characters so SC will still be O(1)

## Code ->
```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        multiset <char> st;
        multiset <char> st2;
        if(s.size() != t.size()) return false;
        for(int i=0; i<s.size(); i++){
            st.insert(s[i]);
        }
        for(int i=0; i<t.size(); i++){
            st2.insert(t[i]);
        }
        
        if(st == st2) return true;
        else return false;
    }
};
```

# [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/description/)

## Code ->
```cpp
class Solution {
public:
    string frequencySort(string s) {
        // Create a vector of pairs to store the frequency and corresponding ASCII character
        vector<pair<int, char>> vec(256);

        // Initialize the vector with zeros and ASCII characters
        for(int i = 0; i < 256; i++){
            vec[i].first = 0;
            vec[i].second = i;
        }

        // Update the frequency of each character in the string
        for(int i = 0; i < s.size(); i++){
            int pos = s[i];
            vec[pos].first++;
        }

        // Sort the vector in descending order based on frequency
        sort(vec.begin(), vec.end(), greater<>());

        // Construct the sorted string based on the sorted vector
        string ans = "";
        for(int i = 0; i < 256; i++){
            // Break if the frequency is zero, as the rest of the elements will also be zero
            if(vec[i].first == 0) break;

            int sz = vec[i].first;
            char ch = vec[i].second;
            while(sz--){
                ans += ch;
            }
        }
        return ans;
    }
};
```

# [1614. Maximum Nesting Depth of the Parentheses](https://leetcode.com/problems/maximum-nesting-depth-of-the-parentheses/submissions/1185933324/)

## Approaches ->
1. Using stack
2. Using simple cnt variable.

## Code ->
```cpp
#include <bits/stdc++.h>
class Solution {
public:
    int maxDepth(string s) {
        int cnt = 0;
        int ans = 0;

        for(int i=0; i<s.size(); i++){
            if(s[i]=='('){
                cnt++;
            }
            else if(s[i]==')'){
                cnt--;
            }

            ans = max(ans, cnt);
        }
        return ans;
    }
};
```

# [13. Roman to Integer](https://leetcode.com/problems/roman-to-integer/description/)

## Code ->
```cpp
class Solution {
public:
    int romanToInt(string s) 
    {
        unordered_map<char, int> T = { { 'I' , 1 },
                                    { 'V' , 5 },
                                    { 'X' , 10 },
                                    { 'L' , 50 },
                                    { 'C' , 100 },
                                    { 'D' , 500 },
                                    { 'M' , 1000 } };
                                    
    int sum = T[s.back()];
    for (int i = s.length() - 2; i >= 0; --i) 
    {
        if (T[s[i]] < T[s[i + 1]])
            sum -= T[s[i]];
        
        else
            sum += T[s[i]];
    }
    
    return sum;
    }
};
```

# [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/description/)

## Approach ->
Pretty simple using dp. We can do it without dp as well but would be pretty simple then

## Code ->
```cpp
class Solution {
public:
    int t[1001][1001]; // Memoization table for dynamic programming

    // Function to check if a substring is a palindrome
    // It was giving me memory limit exc. bc I didn't pass s using reference.
    bool ifPalindrome(string &s, int st, int en){
        if(st >= en) return 1; // Base case: single character is a palindrome

        if(t[st][en] != -1) return t[st][en]; // Check if the result is already memoized

        // Check if the current characters match and recurse for the inner substring
        if(s[st] == s[en])
            return t[st][en] = ifPalindrome(s, st+1, en-1);

        return t[st][en] = 0; // Mark the result as false if the characters don't match
    }

    // Function to find the longest palindromic substring in the given string
    string longestPalindrome(string s) {
        int n = s.size(); 
        memset(t, -1, sizeof(t)); // Initialize memoization table with -1
        int maxLen = INT_MIN; // Variable to store the length of the longest palindromic substring
        int st = 0; // Variable to store the starting index of the longest palindromic substring

        // Nested loops to iterate through all possible substrings
        for(int i = 0; i < n; i++){
            for(int j = i; j < n; j++){
                // Check if the current substring is a palindrome using dynamic programming
                if(ifPalindrome(s, i, j)){
                    // If the length of the current palindrome is greater than the current maxLen
                    if(maxLen < (j - i + 1)){
                        maxLen = (j - i + 1); // Update maxLen
                        st = i; // Update the starting index of the longest palindromic substring
                    }
                }
            }
        }

        // Return the longest palindromic substring using the starting index and length
        return s.substr(st, maxLen);
    }
};
```

