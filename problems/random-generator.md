```python

import time

class LCGRandom:
    def __init__(self, seed=None):
        # Constants for the LCG (these are common values for LCG)
        self.a = 1664525
        self.c = 1013904223
        self.m = 2**32
        
        # Initialize the seed
        if seed is None:
            seed = int(time.time_ns())  # Use current time in milliseconds as seed
        self.state = seed % self.m

    def next(self):
        # Generate the next random number
        self.state = (self.a * self.state + self.c) % self.m
        return self.state

def generate_random_number(L, R):
    """
    Generate a random number within the range (L, R) with an almost uniform probability.
    
    Parameters:
    L (float): The lower bound of the range.
    R (float): The upper bound of the range.
    
    Returns:
    float: A random number within the specified range.
    """
    if L >= R:
        raise ValueError("The lower bound L must be less than the upper bound R.")
    
    lcg = LCGRandom()
    rand_num = lcg.next() / lcg.m  # Normalize to [0, 1)
    return L + (R - L) * rand_num  # Scale to [L, R)

# Example usage
L = 10
R = 20
random_number = generate_random_number(L, R)
print(f"Random number between {L} and {R}: {random_number}")
random_number = generate_random_number(L, R)
print(f"Random number between {L} and {R}: {random_number}")
random_number = generate_random_number(L, R)
print(f"Random number between {L} and {R}: {random_number}")
random_number = generate_random_number(L, R)
print(f"Random number between {L} and {R}: {random_number}")
random_number = generate_random_number(L, R)
print(f"Random number between {L} and {R}: {random_number}")

```
