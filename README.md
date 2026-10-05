def knapsack(products, capacity):
    n = len(products)

    # dp[i][w] = maximum benefit using first i products
    # with storage capacity w
    dp = [[0 for _ in range(capacity + 1)] for _ in range(n + 1)]

    for i in range(1, n + 1):
        name, space, benefit = products[i - 1]

        for w in range(capacity + 1):

            if space <= w:
                dp[i][w] = max(
                    dp[i - 1][w],
                    benefit + dp[i - 1][w - space]
                )
            else:
                dp[i][w] = dp[i - 1][w]

    # Find selected products
    selected = []
    w = capacity

    for i in range(n, 0, -1):
        if dp[i][w] != dp[i - 1][w]:
            name, space, benefit = products[i - 1]
            selected.append(products[i - 1])
            w -= space

    selected.reverse()

    return dp[n][capacity], selected


# Number of products
n = int(input("Enter number of products: "))

products = []

for i in range(n):
    name = input("Enter product name: ")
    space = int(input("Enter storage space: "))
    benefit = int(input("Enter expected benefit: "))

    products.append((name, space, benefit))


# Storage capacity
capacity = int(input("Enter storage capacity: "))


# Apply 0/1 Knapsack
max_benefit, selected_products = knapsack(products, capacity)


print("\nSelected Products:")
total_space = 0

for product in selected_products:
    name, space, benefit = product
    print(name, "- Space:", space, "Benefit:", benefit)
    total_space += space

print("\nTotal Storage Used:", total_space)
print("Maximum Expected Benefit:", max_benefit)
