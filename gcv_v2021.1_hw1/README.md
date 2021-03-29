# Homework 1

Homework for GCV course 2021

## Data

To download data for validation use [this](https://www.dropbox.com/s/lxg7lb8xqcmxowa/validation.zip?dl=0).


## func: get_view() 

```python

pose_i = CameraPose(extrinsics[i])
    
imaging_i = RaycastingImaging(intrinsics_dict[i]['resolution_image'], 
intrinsics_dict[i]['resolution_3d'])
    
points_i = pose_i.camera_to_world(imaging_i.image_to_points(image_i))
```
1.1 Using CameraPose() for transform points to camera and world coordinate frames.

1.2 Using RaycastingImaging() to transform from
picture coordinates to 3d-camera frame coordinates

1.3 Using transform picture distance points to 3d-points in world coordinate 

## func: pairwise_interpolate_predictions()

```python

reprojected_j = pose_i.world_to_camera(points_j)

 uv_i = imaging_i.rays_origins[:, :2]
 kdtree = cKDTree(uv_i)
 distances_nn_in_i, nn_indexes_in_i = kdtree.query(reprojected_j[:, :2], k=nn_set_size)


point_from_j_nns = np.hstack((uv_i[point_nn_indexes], image_i.reshape((-1, 1))[point_nn_indexes]))

distances_to_nearest = np.linalg.norm(point_from_j_nns - point_from_j, axis=1)
interp_mask[idx] = distances_to_nearest.min() < distance_interpolation_threshold

interpolator = interpolate.interp2d(point_from_j_nns[:, 0], point_from_j_nns[:, 1], distances_i.reshape((-1))[point_nn_indexes])
                distances_j_interp[idx] = interpolator(*point_from_j[:2])



```

2.1 Using CameraPose() to transform j 3d-points to i camera coordinate frame.

2.2 Using cKDTree each point from point cloud  find kNN in i point cloud, using cKDTree.

2 .3 Finding distances from each point in j to its nearest neighbours (if d big, et minD < threshold, then pass it)ompare minimal distance to neighbors with some threshold.

2.4 Interpolating distance function in that point, using known distance values in points
