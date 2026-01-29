# What I Learned Today

## 1. Minimum Cost to Convert String I (LeetCode 2976)
- Character conversions can be modeled as a **graph**
- Only **26 lowercase letters**, so all-pairs shortest path is feasible
- Used **Floyd–Warshall** to find minimum conversion cost between characters
- Final answer is the sum of costs for each position
- Return `-1` if any conversion is impossible

## 2. Combine Two Tables (LeetCode 175)
- Required combining two tables while keeping all rows from one table
- Used **LEFT JOIN** in MySQL
- Missing matching rows automatically return `NULL`

## Key Takeaway
- Constraints guide the solution choice
- Graph algorithms and SQL joins solve very different problem types
- Understanding patterns is more important than memorizing solutions
