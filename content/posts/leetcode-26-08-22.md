+++
title = "Daily Leetcode 26th August 2022"
date = "2022-08-26T11:04:07+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "medium", "solution", "rust"]
keywords = ["leetcode", "solution", "jamesm2w"]
description = "Solving today's leetcode problem 869. Reordered Power of 2"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's problem was [869. Reordered Power of 2](https://leetcode.com/problems/reordered-power-of-2/). The basic premise was to return true if the digits of an input integer could be re-arranged into a power of 2. Obviously no leading zeros.

My approach was quite simple, I would create a vector of digits of the input integer, and compare that to the digit vectors of powers of two.

This takes advantage of a static array of all powers of two, since we're using the `i32` datatype and the problem is restricted to `n` in the range `[1, 10^9]` only the first 29 (`log2(10^9)`) powers of two are relevant. I generated these with some python `[2**x for x in range(0, 30)]` and stored that in a static array. 

Then I wrote a helper function to convert an integer `n` into a vector of digits. This was pretty simple, and could probably be expressed more succintly, but it's essentially just repeatedly dividing by 10 and adding the remainder at each pass (the digit) to the vector. 

The actual solution is in `reordered_power_of2` and we can find the window of powers of two we want to check, since a possible match has to have the same number of digits. 

Using rust iterators this is really simple, we can create and filter the iterator for the right window, then check for `any` which match. `any` is short-circuiting so will return `true` for the first one that matches and if none do then `false`.

{{<code language="rust" title="Reordered Power of 2">}}
static powers_of_two: [i32; 30] = [1, 2, 4, 8, 16, 32, 64, 128, 256, 512, 1024, 2048, 4096, 8192, 16384, 32768, 65536, 131072, 262144, 524288, 1048576, 2097152, 4194304, 8388608, 16777216, 33554432, 67108864, 134217728, 268435456, 536870912];
        
impl Solution {
    
    pub fn get_digits(n: i32) -> Vec<i32> {
        let mut digits = Vec::new();
        let mut n = n;
        let mut rem = 0;
        loop {
            rem = n % 10;
            n = n / 10;
            // (n, rem) = (n / 10, n % 10);
            digits.push(rem);
            
            if n == 0 {
                break;
            }
        }
        
        digits
    }
    
    pub fn reordered_power_of2(n: i32) -> bool {
        let mut digits = Solution::get_digits(n);
        digits.sort();
        
        let min = 10i32.pow(digits.len() as u32 - 1);
        let max = 10i32.pow(digits.len() as u32) - 1;
        
        powers_of_two.iter().filter(|x| min <= **x && **x <= max).any(|x| {
            let mut x_digits = Solution::get_digits(*x);
            x_digits.sort();
            digits == x_digits
        })
    }
}
{{</code>}}

This program was very quick, and used very little memory. `get_digits` is `O(log n)` time and space, and the main solution is similarly `O(log n) * O(1)`, the constant comes in from the defined array which gives a max 30 `log n` calls. Similarly sorting can be done in `log n` time so does not add time complexity. The equivalence is linear in the length of the array, but the array could only be as long as the number of digits which is `log n` again!

Quite happy with this solution, let's see what tomorrow brings.