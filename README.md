def nth_fibo(n):
    if n==0: return 0
    if n==1: return 1

   F = [None] * (n + 1)
    F[0] = 0
    F[1] = 1

   for i in range(2, n + 1):
        F[i] = F[i - 1] + F[i - 2]

   return F[n]

n = 6
result = nth_fibo(n)
print(f"The {n}th Fibonacci number is {result}")

def fibonacci_tabulation(n):
    if n == 0: return 0
    elif n == 1: return 1

   F = [0] * (n + 1)
    F[0] = 0 
    F[1] = 1

   for i in range(2, n + 1):
        F[i] = F[i - 1] + F[i - 2]
    
   print(F)
    return F[n]
  
n = 10
result = fibonacci_tabulation(n)
print(f"\nThe {n}th Fibonacci number is {result}")

def F(n):
    if memo[n] != None: # Already computed
        return memo[n]
    else: # Computation needed
        print('Computing F('+str(n)+')')
        if n <= 1:
            memo[n] = n
        else:
            memo[n] = F(n - 1) + F(n - 2)
        return memo[n] 

memo = [None]*7
print('F(6) = ',F(6))
print('memo = ',memo)

computation_count = 0
def F(n):
    global computation_count
    computation_count += 1
    if n <= 1:
        return n
    else:
        return F(n - 1) + F(n - 2)
        
computation_count_mem = 0
def F_mem(n):
    if memo[n] != None: # Already computed
        return memo[n]
    else: # Computation needed
        global computation_count_mem
        computation_count_mem += 1
        if n <= 1:
            memo[n] = n
        else:
            memo[n] = F_mem(n - 1) + F_mem(n - 2)
        return memo[n] 

print('F(30) = ',F(30))
print(f'Number of computations: {computation_count}')
print('\nUsing memoization:')
memo = [None]*31
print('F(30) = ',F_mem(30))
print(f'Number of computations with memoiztion: {computation_count_mem}')

def knapsack_memoization(capacity, n):
    print(f"knapsack_memoization({n}, {capacity})")
    if memo[n][capacity] is not None:
        print(f"Using memo for ({n}, {capacity})")
        return memo[n][capacity]
    
   if n == 0 or capacity == 0:
        result = 0
    elif weights[n-1] > capacity:
        result = knapsack_memoization(capacity, n-1)
    else:
        include_item = values[n-1] + knapsack_memoization(capacity-weights[n-1], n-1)
        exclude_item = knapsack_memoization(capacity, n-1)
        result = max(include_item, exclude_item)

   memo[n][capacity] = result
    return result

values = [300, 200, 400, 500]
weights = [2, 1, 5, 3]
capacity = 10
n = len(values)

memo = [[None]*(capacity + 1) for _ in range(n + 1)]

print("\nMaximum value in Knapsack =", knapsack_memoization(capacity, n))

def knapsack_tabulation():
    n = len(values)
    tab = [[0] * (capacity + 1) for _ in range(n + 1)]

   for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            if weights[i-1] <= w:
                include_item = values[i-1] + tab[i-1][w - weights[i-1]]
                exclude_item = tab[i-1][w]
                tab[i][w] = max(include_item, exclude_item)
            else:
                tab[i][w] = tab[i-1][w]

  for row in tab:
        print(row)

   items_included = []
    w = capacity
    for i in range(n, 0, -1):
        if tab[i][w] != tab[i-1][w]:
            items_included.append(i-1)
            w -= weights[i-1]

  print("\nItems included:", items_included)

  return tab[n][capacity]

values = [300, 200, 400, 500]
weights = [2, 1, 5, 3]
capacity = 10
print("\nMaximum value in Knapsack =", knapsack_tabulation())

class Node:
    def __init__(self, char=None, freq=0):
        self.char = char
        self.freq = freq
        self.left = None
        self.right = None

nodes = []

def calculate_frequencies(word):
    frequencies = {}
    for char in word:
        if char not in frequencies:
            freq = word.count(char)
            frequencies[char] = freq
            nodes.append(Node(char, freq))

def build_huffman_tree():
    while len(nodes) > 1:
        nodes.sort(key=lambda x: x.freq)
        left = nodes.pop(0)
        right = nodes.pop(0)
        
   merged = Node(freq=left.freq + right.freq)
   merged.left = left
   merged.right = right
        
   nodes.append(merged)

   return nodes[0]

def generate_huffman_codes(node, current_code, codes):
    if node is None:
        return

   if node.char is not None:
        codes[node.char] = current_code

   generate_huffman_codes(node.left, current_code + '0', codes)
   generate_huffman_codes(node.right, current_code + '1', codes)

def huffman_encoding(word):
    global nodes
    nodes = []
    calculate_frequencies(word)
    root = build_huffman_tree()
    codes = {}
    generate_huffman_codes(root, '', codes)
    return codes

def huffman_decoding(encoded_word, codes):
    current_code = ''
    decoded_chars = []

   # Invert the codes dictionary to get the reverse mapping
   code_to_char = {v: k for k, v in codes.items()}

   for bit in encoded_word:
        current_code += bit
        if current_code in code_to_char:
            decoded_chars.append(code_to_char[current_code])
            current_code = ''

   return ''.join(decoded_chars)

word = "lossless"
codes = huffman_encoding(word)
encoded_word = ''.join(codes[char] for char in word)
decoded_word = huffman_decoding(encoded_word, codes)

print("Initial word:", word)
print("Huffman code:", encoded_word)
print("Conversion table:", codes)
print("Decoded word:", decoded_word)
