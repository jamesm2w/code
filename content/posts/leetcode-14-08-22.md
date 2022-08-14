+++
title = "Daily Leetcode 14th August 2022"
date = "2022-08-14T14:36:42+01:00"
author = "jamesm2w"
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode", "hard", "solution", "rust"]
keywords = ["ihatemylife", "leetcode", "solution"]
description = "Solving today's leetcode problem 126. Word Ladder II in Rust"
showFullContent = false
readingTime = false
hideComments = false
+++

Today's problem was number [126. Word Ladder II](https://leetcode.com/problems/word-ladder-ii/)

My initial attempt pasted below was to perform a graph search for the path(s) to the target. Using words as nodes, and each node was connected to every other node s.t. their characters only differed by 1. This solution worked very well, but the time complexity of the solution is very high, approximately like `O(b^n)` where `b` is the forward branching factor and `n` is the depth of the paths. 

To say nothing of the spatial complexity, which as it's storing the whole path multiple times is exponentially large. 
That said this solution did work, but timed out on some of the larger test cases, to remedy this I added everything from cycle-checking, muti-path-pruning to cut down on the number of choices available at one time. 

This improved the average efficiency somewhat, and allowed it to past most of the testcases but there were still some issues with the ~500 size word lists. I tested numerous versions with heuristics, using heaps etc to give something like Best First Search, however it was not enough. 

{{<code language="rust" title="Word Ladder II">}}
// Basic gist here
// Can't promise this works but you get the general idea of what I was trying
impl Solution {
    pub fn find_ladders(begin_word: String, end_word: String, word_list: Vec<String>) -> Vec<Vec<String>> {
        
        // No paths will ever be possible in this case
        if !word_list.contains(&end_word) {
            return vec![];
        }
        
        let mut successor_words: HashMap<&String, Vec<&String>> = HashMap::new();

        for word in word_list.iter() {
            let succs = Solution::generate_successor_words(word.as_str(), &word_list);
            successor_words.insert(word, succs);
        }
        successor_words.insert(&begin_word, Solution::generate_successor_words(begin_word.as_str(), &word_list));

        // Perform some BFS from start following succs until goal is reached
        // then set limit at goal depth + continue until all paths are exhausted
       
        let mut paths: Vec<Vec<String>> = Vec::new();
        
        let mut frontier: BinaryHeap<Vec<&String>> = BinaryHeap::new();
        frontier.push(vec![&begin_word]);
        
        let mut explored_set: HashSet<&String> = HashSet::new();
        let mut max_depth = usize::MAX;
        
        while let Some(path) = frontier.pop() {
            let last_element = path.0.last().unwrap();
            
            if path.0.len() > max_depth {
                println!("\tbeyond max depth");
                continue;
            }
            
            if *last_element == &end_word {
                paths.push(path.iter().cloned().map(|e| e.to_string()).collect::<Vec<String>>());
                max_depth = path.len();
                continue;
            }
            
            explored_set.insert(*last_element);
            let succ_words = successor_words.get(last_element).unwrap();
            
            // Cycle Checking and Multi-Path Pruning: Do not expand to nodes which are on the path or have been placed in the explored set
            // since we already have paths to them (which would probably be shorter)
            for word in succ_words.iter().filter(|e| !explored_set.contains(*e)) {
                let mut new_path = path.0.clone();
                new_path.push(word);
                
                if new_path.len() < max_depth {
                    frontier.push(Reverse(new_path));
                }
            }
        }
        
        paths
    }
    
    pub fn compare_words(word_a: &str, word_b: &str) -> i32 {
        word_a.chars().zip(word_b.chars()).map(|(a, b)| a == b).fold(0, |acc, m| acc + if m { 0 } else { 1 })
    }
    
    pub fn generate_successor_words<'a>(word: &str, word_list: &'a Vec<String>) -> Vec<&'a String> {
        word_list.iter().filter(|w| Solution::compare_words(word, w) == 1).collect::<Vec<_>>()
    }
}
{{</code>}}

An approach based on a backtracking search would have worked better here I think, and in future I would go to that sooner, since it also probably has a decent average time complexity over just straight Breadth First searching.

Other options I didn't try were branch/bounded DFS approaches, or more concrete heuristic searches, e.g. A*. One issue I can see with such heurisitc searches is that they could run into issues with the small word length in this problem which was bounded between `0` and `5`, meaning a heurisitc such as "distance from target" might not be that useful given it could only split the `500` or so words into 5 equiv categories.

Another hard problem today, but I enjoyed trying it. Let's see what tomorrow brings.