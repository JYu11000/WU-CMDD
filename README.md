# WU-CMDD

[![CC BY 4.0][cc-by-shield]][cc-by]

[![Data Schema](https://img.shields.io/badge/Data-Event_Camera%7CLiDAR%7CRGB%7CCAN-brightgreen)](https://github.com/your-repo)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.xxxxxx.svg)](https://doi.org/your-doi)

[cc-by]: http://creativecommons.org/licenses/by/4.0/
[cc-by-shield]: https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg

## License
This dataset is licensed under a  
[Creative Commons Attribution 4.0 International License][cc-by].

**You must:**
- 📌 **Cite our paper** (see [CITATION](CITATION.md))  
- 🔗 **Link to this repository** in derivative works  
- ⚠️ **Clearly indicate modifications** if you adapt the data  

## 1. Dataset Overview
The **Westlake-Campus Multimodal Driving Dataset (WU-CMMD)** is a comprehensive urban driving dataset captured on campus roads between April and June 2025. With a total size of 348GB, it contains synchronized multimodal sensor data including:

- **Event camera images** (.png format)
- **RGB camera images** (.png format)
- **LiDAR point clouds** (.bin format)
- **Vehicle state data** (ROS `geometry_msgs/TwistStamped` messages in .txt format)

This dataset supports research in autonomous driving, sensor fusion, and vehicle dynamics analysis.

## 2. Dataset Access
### Download Instructions
Clone the repository with Git:
```bash
git clone https://github.com/your-username/WU-CMDD.git
```

## 3. Data Format Description
1. **Camera Image Data**
   - **Event Image**: Stored in .png format, records event information captured by the camera.
   - **RGB Image**: Also stored in .png format, presents color images of road scenes.

2. **Radar Data**
   - Radar data is point cloud data stored in .bin file format, containing 3D information of objects around the road.

3. **Vehicle Status Data**
   - Vehicle status data records ROS topics of type geometry_msgs/TwistStamped, stored in .txt files, which can be used to understand the vehicle's motion status.

## 4. Data Usage Examples

### Reading Radar Data
For radar .bin files, you can use the following Python code example for reading (assuming NumPy library is used):
```python
import numpy as np

# Read .bin file
point_cloud = np.fromfile('radar_point_cloud.bin', dtype=np.float32)

# Data processing can be added here according to actual needs
print(point_cloud)
```

### Reading Vehicle Status Data
For vehicle status .txt files, you can use the following Python code example for reading:
```python
import os
import yaml

def parse_one(path):
    with open(path, 'r', encoding='utf-8') as f:
        data = yaml.safe_load(f)
    
    # Extract timestamp information
    stamp = data['header']['stamp']
    ts = float(stamp['secs']) + 1e-9 * stamp['nsecs']
    
    # Extract vehicle state information
    speed = data['twist']['linear']['x']
    steer_z = data['twist']['angular']['x']
    
    # Determine steering direction
    direction = "left" if steer_z > 0 else "right" if steer_z < 0 else "go straight"
    
    # Print formatted output
    print(f"{os.path.basename(path):<25s} "
          f"ts={ts:<15.3f} "
          f"speed={speed:7.3f} m/s "
          f"steer={steer_z:8.3f} deg ({direction})")

parse_one('vehicle_state.txt')
```

Install dependencies (only PyYAML required):
```
pip install pyyaml
```

Please note that the above examples are for demonstration purposes only and may need to be modified and extended according to specific requirements in practical applications.
