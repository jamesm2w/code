+++
title = "Daily Leetcode 13th August 2022"
date = "2022-08-13T11:06:18+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
tags = ["leetcode", "hard", "solution", "rust"]
keywords = ["i hate my life", "leetcode", "leetcode solution"]
description = "Daily Problem, Solving Leetcode 30. Substring with Concatenation of All Words using Rust"
showFullContent = false
readingTime = true
hideComments = false
+++

Today's leetcode daily problem was [30. Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/) (Difficulty: Hard). The problem was basically as it sounds, given a string and a vector of words, find the starting indicies of all substrings which are an `n` permutation of the `n` words concat together with no gaps or inbetween chars, etc.

A brute force approach would give about `n*n!` comparisons as `nPn = n!` and you'd probably have to compare each character.

Here's my initial naive approach, which tries to loop through the word and checks if each substring at start..start + substr length is a full match or not. Utilising HashMap for a quick check for membership, although there may be more efficient ways to get that given overhead for hashing the String structure. 

It runs quite slowly (~700ms when I ran it), and there could be some optimisation of how the map is created (I imagine calling that multiple times isn't that efficient on large arrays.) Similarly, the assignment/reading from the map could be more rust-like I feel.

{{<code language="rust" title="Substring with Concatenation of All Words">}}
use std::collections::HashMap;

impl Solution {

// Helper to reset the map to have all the words
fn reset_hash_map(words: &Vec<String>) -> HashMap<String, usize> {
    let mut map = HashMap::new();
    for word in words.iter().cloned() {
        match map.get(&word) {
            Some(value) => { map.insert(word, value + 1); },
            None => { map.insert(word, 1); }
        };
    }
    
    map
}

// Helper to just quickly get the sum of all keys
#[inline]
fn words_in_map(map: &HashMap<String, usize>) -> usize {
    map.values().fold(0, |acc, x| acc + x)
}

pub fn find_substring(s: String, words: Vec<String>) -> Vec<i32> {

    let mut words_set = Solution::reset_hash_map(&words);

    let mut returnv: Vec<i32> = Vec::new();
    let wordlen = words[0].len();
    let wordslen = words.len();
    // max_len = s.len() - (Solution::words_in_map(words_set) * wordlen); 
    let mut index = 0;
    loop {
        
        // Like this shouldn't really need to be checked one every iteration
        if Solution::words_in_map(&words_set) * wordlen > s.len() {
            break
        }
        
        // Matched all words so this is a full substring of all words concat
        if Solution::words_in_map(&words_set) == 0 {
            
            // Push the start index onto the return vector
            returnv.push((index - (wordslen * wordlen)).try_into().unwrap());

            // Reset the word set/map to contain all words again
            words_set = Solution::reset_hash_map(&words);

            // Move index back to the next char after the first in the substr (for overlapping matches)
            index = index - (wordslen * wordlen) + 1;

        }
        
        if index > s.len() - (Solution::words_in_map(&words_set) * wordlen) { // Stop if there's not enough space to get the rest of the words out
            break
        } else {
            let slice = s.get(index..(index+wordlen));
            
            match slice {
                // Matches if valid index
                Some(substr) => match words_set.get(substr) {
                    // An exhausted word (count: 0) and None are basically equiv (a failed match)
                    Some(0) | None => {
                        let found_words = wordslen - Solution::words_in_map(&words_set);
                        
                        // Reset index back to before the found words started, +1 to check next char
                        index = index - (found_words * wordlen) + 1;

                        // Reset the word map to default 
                        words_set = Solution::reset_hash_map(&words);
                    },

                    // Matched the word to a key in the dict with a non-zero count
                    Some(val) => {
                        // Remove a count from the word in the map
                        words_set.insert(substr.to_string(), val - 1);
                        
                        // Move index on to match after this word
                        index = index + wordlen;
                    }
                },
                // Can ignore other cases (None shouldnt occur), but need to increment index ig
                _ => { index = index + 1; }
            }
        }
    }
    returnv
}
}
{{</code>}}

Overall, I felt this one was quite difficult. Had some annoying oversights on corner cases (e.g. words arr was longer than the string) which caused some panic!. Glad to have finished it, and probably wont be improving on the efficiency unless I have to solve it again.