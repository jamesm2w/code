+++
title = "Daily Leetcode 21st August 2022"
date = "2022-08-21T12:17:56+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "hard", "solution", "rust"]
keywords = ["ihatemylife", "leetcode", "solution"]
description = "Solving today's leetcode problem 936. Stamping The Sequence"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's problem was [936. Stamping The Sequence](https://leetcode.com/problems/stamping-the-sequence/). The basic premise was to find an order of "stamps" of a given string to result in the target string. A stamp operation was replacing characters from `i` to `i+stamp` with the stamp characters. Also the target string was fixed length so you can't stamp over the edge.

I found this quite a difficult challenge to approach, initially I tried out some forward searching methods but quickly felt they wouldn't be anywhere near efficient enough. So instead I did a bit of reading and decided to approach the problem from the other side, working backwards from the target to undo the stamping operations, then reverse that order to get the correct stamp order.

The basic idea is for at a maximum of `10 * len` passes look at the stamp-length windows of the target (which I converted to a simple vec of chars for ease of manipulation) and if any match the stamp or partially match the stamp (allowing for some `?`) then note that index, replace the stamp with `?` and continue.

If we get to a point where the entire string is `?` we've found a solution and can return the reverse of that list, else if we run out of iterations we can just return an empty vector.

{{<code language="rust" title="Stamping the Sequence">}}
impl Solution {
pub fn moves_to_stamp(stamp: String, target: String) -> Vec<i32> {

        let mut stamp: Vec<char> = stamp.chars().collect();
        let mut target: Vec<char> = target.chars().collect(); 
        let mut ret: Vec<i32> = Vec::new();
        
        // do max 10*len times
        // find a substring in target which == stamp
        //     replace substring with ???, not starting index of substring and add to return vec
        //  repeat but allow substring matches to match with ? or the char
        //  if we get to a full ???? string then reverse the order of the indicies et voila
        for _ in 0..10*target.len() {
            for (i, w) in target.windows(stamp.len()).enumerate() {
                if w.iter().zip(stamp.iter()).all(|(wc, sc)| wc == sc || wc == &'?' ) && !w.iter().all(|x| x == &'?')  {
                    ret.push(i as _);
                    
                    for (j, char) in stamp.iter().enumerate().map(|(j, char)| { (j + i, char) }) {
                        target[j] = '?';
                    }
                    
                    // Only do one replacement each cycle to ensure we don't go above 10*len
                    break;
                }
            }
            
            // end early if replaced all already
            if target.iter().all(|x| x == &'?') {
                break;
            }
        }
        
        // just check if we found a solution or have to return the empty list
        if target.iter().all(|x| x == &'?') {
            ret.reverse();
            ret
        } else {
            vec![]
        }
        
    }
}
{{</code>}}

The time of this implementation is probably pretty poor, but I'd still think it's polynomial at least. Storage wise it's quite efficient using in-place manipulations and iterators where possible -- although that probably feeds into the higher execution time.

Sadly I missed a few of the previous challenges because I couldn't find a solution I was happy with in the day, but we move. Lets see what tomorrow's is (thinking it's gonna be an easy?).