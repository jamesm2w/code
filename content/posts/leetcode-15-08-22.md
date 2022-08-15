+++
title = "Daily Leetcode 15th August 2022"
date = "2022-08-15T09:36:51+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "easy", "solution", "rust"]
keywords = ["ihatemylife", "leetcode", "solution"]
description = "Solving today's leetcode problem 13. Roman to Integer"
showFullContent = false
readingTime = true
hideComments = false
+++

Todays problem was [13. Roman to Integer](https://leetcode.com/problems/roman-to-integer/)

Easy/simple problem today, this was 13. Roman to Integer which given a string of a roman numeral convert it into an integer value. Fairly simple to write a linear traversal through the the chars and using some map func of the numerals to their values add to an accumulator.

This solution was pretty fast, but probably not the most idiomatic rust solution (I'm using unwraps, etc). A better way might be to utilise the functional iterators better with a fold or reduction from right to left, accumulating all the chars to their integer values and depending on how big the accumulator is you'd know whether to add or subtract a smaller value.

{{<code language="rust" title="Roman to Integer">}}
impl Solution {
    pub fn roman_to_int(s: String) -> i32 {
        let values: HashMap<char, _> = HashMap::from_iter([
            ('I',    1), 
            ('V',    5), 
            ('X',   10), 
            ('L',   50), 
            ('C',  100), 
            ('D',  500), 
            ('M', 1000)
        ]);
        
        let mut val = 0;
        let mut chars = s.chars().peekable();
        
        while let Some(cchar) = chars.next() {
            
            let nchar = chars.peek().unwrap_or(&' ');
            let cchar_val = values.get(&cchar).unwrap_or(&0);
            let nchar_val = values.get(&nchar).unwrap_or(&0);
            
            if cchar_val < nchar_val {

                val -= cchar_val;
            } else {

                val += cchar_val;
            }
            
        }
        
        val
    }
}
{{</code>}}

Either way this was a quick one and the solution was quite fast, cant complain there!