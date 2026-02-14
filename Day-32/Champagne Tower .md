# 799. Champagne Tower 

## Problem Summary
We have a pyramid of glasses where each glass can hold exactly **1 unit** of champagne. 
- When a glass is full, any extra liquid overflows and falls **equally** (50/50) into the two glasses directly below it.
- Given the total amount of champagne poured at the very top, we need to determine how full a specific glass at `(query_row, query_glass)` is.

---

## The Strategy: Row-by-Row Simulation
Instead of complex math, we simulate the flow of champagne row by row, similar to how values are calculated in Pascal's Triangle.

### 1. The Setup
We create a 2D grid to track the total amount of champagne that passes through each glass. Even though a glass only "holds" 1 unit, we initially let it "store" the total amount poured into it to calculate the overflow.

### 2. The Overflow Logic
For every glass in the tower:
- If the amount in the glass is **greater than 1.0**:
  - Calculate the excess: `overflow = (Total Amount - 1.0)`.
  - Send **half** of that excess to the bottom-left glass.
  - Send the other **half** of that excess to the bottom-right glass.

### 3. The Final Result
After simulating down to the `query_row`, the amount in our target glass might be very high (because it tracks what passed through it). Since a glass can only hold 1 unit, we return `min(1.0, amount)`.

---

## Complexity Analysis
- **Time Complexity:** $O(R^2)$
  - We use a nested loop to iterate through the rows ($R$) and columns. Since $R \le 100$, we perform at most $10,000$ operations.
- **Space Complexity:** $O(R^2)$
  - We store the tower in a 2D array of size $101 \times 101$.

---

##  C++ Implementation

```cpp
#include <vector>
#include <algorithm>

using namespace std;

class Solution {
public:
    double champagneTower(int poured, int query_row, int query_glass) {
        // tower[i][j] stores the amount of champagne poured INTO the glass
        double tower[102][102] = {0.0};
        
        // Pour everything into the top glass first
        tower[0][0] = (double)poured;
        
        // Iterate through each row to distribute the overflow
        for (int i = 0; i <= query_row; ++i) {
            for (int j = 0; j <= i; ++j) {
                // If the glass overflows (> 1 cup)
                if (tower[i][j] > 1.0) {
                    double overflow = (tower[i][j] - 1.0) / 2.0;
                    
                    // Split excess equally between the two glasses below
                    tower[i + 1][j] += overflow;
                    tower[i + 1][j + 1] += overflow;
                }
            }
        }
        
        // Result is capped at 1.0 because a glass can't hold more than its capacity
        return min(1.0, tower[query_row][query_glass]);
    }
};
