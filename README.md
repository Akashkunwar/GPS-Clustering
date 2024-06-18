# GPS Clustering Streamlit App

This Streamlit app allows users to upload a CSV file containing GPS coordinates, specify the number of clusters, and visualize the clustered data on an interactive map. Users can also download the clustered results as a CSV file.

Check out the deployed application: [GPS Clustering Streamlit App](https://gps-clustering.streamlit.app/)

## Features

- Upload a CSV file with GPS coordinates.
- Dynamically adjust the number of clusters.
- Visualize the clusters on an interactive map.
- Download the clustered data as a CSV file.
- Option to show center pins of each cluster on the map.

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/Akashkunwar/gps-clustering-app.git
   cd gps-clustering-app
   ```

2. Install the required packages:
   ```bash
   pip install -r requirements.txt
   ```

3. Run the Streamlit app:
   ```bash
   streamlit run app.py
   ```

## Usage

### Sidebar Inputs

- **Number of clusters to be created**: Use the number input to set the initial number of clusters.
- **Adjust no. of Cluster**: Use the slider to dynamically adjust the number of clusters.
- **Upload CSV file with GPS Coordinates**: Upload your CSV file containing the GPS coordinates.
- **Show centre pin of each cluster**: Checkbox to toggle the display of center pins for each cluster.

### Sample CSV

Download the sample CSV to understand the required format:
```python
csv = pd.read_csv('Sample.csv')

st.download_button(
    label="Download Sample CSV",
    data=csv.to_csv(),
    file_name='Sample.csv',
    mime='text/csv',
)
```

### File Upload and Processing

After uploading the CSV file, the app processes the data and applies the K-Means clustering algorithm:
```python
if uploaded_file is not None:
    try:
        df = pd.read_csv(uploaded_file)
        df[['Latitude', 'Longitude']] = df['GPS'].str.split(',', expand=True).astype(float)
        df = df[['ID', 'Latitude', 'Longitude']]
    except:
        st.error("Sheet is in wrong format")
```

### Clustering and Visualization

The K-Means clustering algorithm is used to group the GPS coordinates into the specified number of clusters. The results are displayed on an interactive Folium map:
```python
try:
    kmeans = KMeans(n_clusters=int(clusterNumber), init='k-means++')
    kmeans.fit(df[df.columns[1:3]])
    df['cluster_label'] = kmeans.fit_predict(df[df.columns[1:3]])
    centers = kmeans.cluster_centers_
    ...
    folium_static(m)
except Exception as e:
    st.warning(f"Something went wrong! Contact Support. Error: {e}")
```

### Download Output

Users can download the clustered data:
```python
st.download_button(
    label="Download Output",
    data=download.to_csv().encode('utf-8'),
    file_name='Clustered_GPS.csv',
    mime='text/csv',
)
```

## Example

1. Set the number of clusters using the sidebar controls.
2. Upload your CSV file containing GPS coordinates.
3. Optionally, check the "Show centre pin of each cluster" to display the cluster centers on the map.
4. Visualize the clusters on the interactive map.
5. Download the clustered data as a CSV file.

## Contributions

Feel free to open issues or submit pull requests if you have suggestions or improvements.

## License

This project is licensed under the MIT License.

---

Explore the app and let me know your feedback! If you have any questions or need further assistance, please contact me.
