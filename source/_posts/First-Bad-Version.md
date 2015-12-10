title: First Bad Version
date:   2015-12-8 15:23:32
categories: algothrim
tags: algothrim
---



 You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool isBadVersion(version) which will return whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API. 


### code:
```cplusplus


// Forward declaration of isBadVersion API.
bool isBadVersion(int version);


class Solution {
  public:
    int firstBadVersion(int n) {
        if (isBadVersion(1)) {
            return 1;
        }

        int l = 1;
        int r = n;
        int found = n;
        while (l <= r) {
            int m = l + (r - l) / 2;
            if (isBadVersion(m)) {
                found = m;
                r = m - 1;
            } else {
                l = m + 1;
            }
        }

        return found;
    }
};
```