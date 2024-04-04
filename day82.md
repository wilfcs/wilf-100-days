201. Bitwise AND of Numbers Range

```cpp
class Solution {
public:
    int rangeBitwiseAnd(int left, int right) {
        int shift = 0;
        
        while (left < right) {
            left >>= 1;
            right >>= 1;
            shift++;
        }
        
        return left << shift;
    }
};
```

67. Add Binary
```cpp
class Solution {
public:
    string addBinary(string a, string b) {
        int size = max(a.size(), b.size());
        reverse(a.begin(), a.end());
        reverse(b.begin(), b.end());
        string ans = "";
        int carry = 0;

        for(int i=0; i<size; i++){
            if(i<a.size() && i<b.size()){
                if(carry == 1){
                    if(a[i]=='1' && b[i]=='1')
                        ans+='1';
                    else if(a[i]=='0' && b[i]=='0'){
                        carry = 0;
                        ans+='1';
                    }
                    else{
                        carry = 1;
                        ans+='0';
                    }
                }
                else{
                    if(a[i]=='1' && b[i]=='1'){
                        ans+='0';
                        carry = 1;
                    }
                       
                    else if(a[i]=='0' && b[i]=='0'){
                        ans+='0';
                    }
                    else{
                        ans+='1';
                    }
                }
            }
            else if(i<a.size()){
                if(carry==1){
                    if(a[i]=='1')
                        ans+='0';
                    else{
                        carry = 0;
                        ans+='1';
                    }
                }
                else{
                    if(a[i]=='1')
                        ans+='1';
                    else
                        ans+='0';
                }

            }
            else{
                if(carry==1){
                    if(b[i]=='1')
                        ans+='0';
                    else{
                        carry = 0;
                        ans+='1';
                    }
                }
                else{
                    if(b[i]=='1')
                        ans+='1';
                    else
                        ans+='0';
                }
            }
        }
        if(carry == 1) ans+='1';
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

1318. Minimum Flips to Make a OR b Equal to c
```cpp
class Solution {
public:
    vector<int> getBit(int n){
        vector<int> v(100,0);
        int i=0;
        while(n){
            v[i++]=n%2;
            n/=2;
        }
        reverse(v.begin(),v.end());
        return v;
    }
    int minFlips(int a, int b, int c) {
        vector<int> v1 = getBit(a);
        vector<int> v2 = getBit(b);
        vector<int> v3 = getBit(c);
        int cnt=0;
        
        int i=v1.size()-1,j=v2.size()-1,k=v3.size()-1;
        while(i>=0&&j>=0&&k>=0){
            if(v3[k]==1){
                if(v2[j]==0&&v1[i]==0){
                    cnt++;
                }
            }else{
                 if(v2[j]==1&&v1[i]==1){
                    cnt+=2;
                }
                 else if(v2[j]==1||v1[i]==1){
                    cnt++;
                }
            }
            i--;
            j--;
            k--;
        }
        return cnt;
    }
};
```

149. Max Points on a Line
```cpp
class Solution {
public:
    int maxPoints(vector<vector<int>>& points) {
        int n=points.size(),ans=1;
        for(int i=0;i<n-1;i++)
        {
            unordered_map<double,int>m;
            for(int j=i+1;j<n;j++)
            {
                if(points[i][0]==points[j][0])
                {
                    m[1000001]++;
                }
                else
                {
                    double slope=(double)(points[j][1]-points[i][1])/(double)(points[j][0]-points[i][0]);
                    m[slope]++;
                }
            }
            int temp=INT_MIN;
            for(auto i:m)
            {
                temp=max(temp,i.second);
            }
            ans=max(ans,temp+1);
        }
        return ans;
    }
};
```

System design - 

Content Delivery Networks (CDNs)
- Discusses using a CDN to efficiently serve static content (HTML pages, images, etc.) to geographically distributed users
- Main points:
    - Cache static content on a global cache/CDN instead of serving from origin server directly
    - Shard/split the CDN cache based on user locations to serve localized content 
    - Place caching nodes/PoPs (Points of Presence) closer to user locations to reduce latency
    - Use commercial CDN providers like Akamai instead of building your own CDN
    - Benefits: Caching, reduced latency, localized content customization

Single Points of Failure
- Discusses identifying and mitigating single points of failure (SPOFs) in distributed systems
- Main points:
    - SPOFs are components which can bring down the entire system if they fail
    - Add redundancy (backup nodes) to remove SPOFs 
    - Use master-slave/replication for databases to avoid SPOF
    - Load balancers themselves are SPOFs, so use multiple LBs
    - Use multiple DNS entries to avoid DNS being a SPOF
    - Deploy system across multiple regions/availability zones 
    - Services like Netflix's Chaos Monkey randomly kill nodes to test resiliency
