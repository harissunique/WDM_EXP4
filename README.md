### EX4 Implementation of Cluster and Visitor Segmentation for Navigation patterns
### DATE: 27/08/26
### AIM: To implement Cluster and Visitor Segmentation for Navigation patterns in Python.
### Description:
<div align= "justify">Cluster visitor segmentation refers to the process of grouping or categorizing visitors to a website, 
  application, or physical location into distinct clusters or segments based on various characteristics or behaviors they exhibit. 
  This segmentation allows businesses or organizations to better understand their audience and tailor their strategies, marketing efforts, 
  or services to meet the specific needs and preferences of each cluster.</div>
  
### Procedure:
1) Read the CSV file: Use pd.read_csv to load the CSV file into a pandas DataFrame.
2) Define Age Groups by creating a dictionary containing age group conditions using Boolean conditions.
3) Segment Visitors by iterating through the dictionary and filter the visitors into respective age groups.
4) Visualize the result using matplotlib.

### Program:
```python
import pandas as pd
import matplotlib.pyplot as plt

# Read CSV file
df = pd.read_csv("clustervisitor.csv")

# Display the visitor dataset
print("Visitor Dataset:")
print(df)

# Select the Age feature
ages = df["Age"].tolist()

# Number of clusters
K = 3

# Initial centroids
centroids = [
    min(ages),
    sum(ages) / len(ages),
    max(ages)
]

# Repeat clustering process
for iteration in range(10):

    clusters = [[], [], []]

    # Assign each visitor to the nearest centroid
    for age in ages:

        distances = [
            abs(age - centroid)
            for centroid in centroids
        ]

        nearest_cluster = distances.index(
            min(distances)
        )

        clusters[nearest_cluster].append(age)

    # Calculate new centroids
    new_centroids = []

    for i in range(K):

        if len(clusters[i]) > 0:

            new_centroid = (
                sum(clusters[i])
                / len(clusters[i])
            )

        else:

            new_centroid = centroids[i]

        new_centroids.append(new_centroid)

    # Stop when centroids do not change
    if new_centroids == centroids:
        break

    centroids = new_centroids


# Assign final cluster labels to visitors
visitor_clusters = []

for age in ages:

    distances = [
        abs(age - centroid)
        for centroid in centroids
    ]

    nearest_cluster = distances.index(
        min(distances)
    )

    visitor_clusters.append(
        nearest_cluster
    )


# Add cluster labels to the DataFrame
df["Cluster"] = visitor_clusters


# Display visitor details with cluster labels
print("\nVisitor Details with Clusters:")
print(df)


# Display cluster-wise visitor details
for i in range(K):

    print(f"\nVisitors in Cluster {i}:")

    print(
        df[df["Cluster"] == i]
    )


# Display final centroids
print("\nFinal Centroids:")

for i in range(K):

    print(
        f"Cluster {i}: "
        f"{centroids[i]:.2f}"
    )


# Visualize the clusters
for i in range(K):

    cluster_data = df[
        df["Cluster"] == i
    ]

    # Plot visitor points
    plt.scatter(
        cluster_data["Age"],
        cluster_data["Cluster"],
        label=f"Cluster {i}",
        s=100
    )

    # Display the age value above each dot
    for _, row in cluster_data.iterrows():

        plt.annotate(
            str(row["Age"]),
            (
                row["Age"],
                row["Cluster"]
            ),
            textcoords="offset points",
            xytext=(0, 10),
            ha="center"
        )

```

### Visualization:
```python
# Display centroids
plt.scatter(
    centroids,
    [0, 1, 2],
    marker="X",
    s=200,
    label="Centroids"
)


plt.xlabel("Age")
plt.ylabel("Cluster")
plt.title(
    "Visitor Segmentation using K-Means"
)

plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()


```
### Output:
<img width="1076" height="545" alt="image" src="https://github.com/user-attachments/assets/7b974b8c-8af0-4bda-96c9-d3ae3c697a35" />

<img width="472" height="247" alt="image" src="https://github.com/user-attachments/assets/7c375ab3-564b-49e9-927b-cb2915f89f0d" />

<img width="465" height="257" alt="image" src="https://github.com/user-attachments/assets/5e4800dd-ce1f-4437-83b6-36599b31f5a4" />

<img width="415" height="222" alt="image" src="https://github.com/user-attachments/assets/97302a09-f5a4-442c-9646-ffa537702b12" />

<img width="182" height="125" alt="image" src="https://github.com/user-attachments/assets/6b2aaa36-21f7-4a97-b1f0-b7423f7da8c5" />

<img width="1041" height="671" alt="image" src="https://github.com/user-attachments/assets/bb58b9f6-5c53-4043-a1f0-34293aea3c78" />

### Result:
Thus, the K-Means clustering algorithm was successfully implemented, and the visitors were grouped into different clusters and visualized using a scatter plot.
