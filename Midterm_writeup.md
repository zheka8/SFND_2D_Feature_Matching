# Sensor Fusion Camera 2D Feature Tracking Midterm Project Writeup
Eugene Klitenik

## MP.0 Mid-Term Report
This report covers rubric points MP.1 to MP.9, with a separate file (PerformanceEval.txt) provided containing detailed times and statistics.

## MP.1 Data Buffer Optimization
The databuffer is implemented as vector with a limited size. When new images are added to the buffer, a check is performed to determine if the vector exceeds a predetermined size, and if needed, old images are removed. For this projet data buffer size is set to 2 since only 2 images are needed for each iteration of the algorithms. Therefore, only 2 images will be stored in memory at any given time.

## MP.2 Keypoint Detection
Keypoint detection is implemented using the OpenCV library. Various detectors can be selected by passing a string with the detector name to a function which then creates the detector. An overall loop is used to select each detector at a time in order to run through all detectors and compare performance. 

SIFT and FAST are contained in xfeatures2d which must be included to use them. 

Once the detector is selected and created, it is used to perform keypoint detection for the provided image. The keypoints are passed by reference to the detection function. Detection for each image is timed and can also be visualized.

## MP.3 Keypoint Removal
A rectangle is defined which encompasses the vehicle of interest in the frame. Keypoint removal is performed to eliminate all keypoints outside of the defined rectangle. This is implemented by checking each keypoint coordinates and determining if they are inside the rectangle of interest. A new vector is used to store keypoints that are inside the rectangle. Removing unnecessary keypoints not only speeds up computation, but also makes visualization easier to understand. 

## MP.4 Keypoint Descriptors
Similar to keypoint detection, a number of keypoint descriptors are implemented via a selectrable mechanism. The descriptor name is defined via a string and passed to a function which then creates the descriptor appropriately using the OpenCV library. Descriptors are then used in order to compare keypoints between successive frames. This can be used to estimate motion. The descriptor operation is timed for each image.

SIFT, FREAK, and BRIEF are contained in the xfeatures2d which must be included in order to use those detectors.

## MP.5 Descriptor Matching
Matching is performed using either the brute force algorithm or FLANN. FLANN is faster than the brute force method for large images. Due to a bug in OpenCV implementation in this project, it is required to convert binary descriptors to floats. This is perormed using OpenCV's built-int convertTo method on both the source and the destination images.

## MP.6 Descriptor Distance Ratio
K-Nearest-Neighbor matching is implemented using the OpenCV library. A descriptor distance ratio test is used to compare the top 2 matches. This test determines whether to keep or discard the associated pair of keypoints. A minimum ratio is set to 0.8. For each KNN match, the best and second-best match are compared, and the match is only kept if the minimum ratio condition is satsified.

## MP.7-9 Performance Evaluation
Each combination of descriptor and detector is evaluated. This is implemented by wrapping most of the provided code in the main function in a double loop, one for the descriptor and one for the detector. A vector of strings is created for each with the possible choices. The algorithm is then run and statistics are written to file for comparison. Tabular results are saved in PerformanceEval.csv.

### MP.7 Performance Evaluation 1
The number of keypoints on the preceding vehicle is summed up for all 10 images for all detectors. Results are located in the PerformanceEval.txt and .csv file. This is used in combination with overall run time to determine the best combination of descriptor and detector.

### MP.8 Performance Evaluation 2
The number of matched keypoints for all 10 images is summed by for each detector/descriptor combination. A brute force matching method is used. Results are located in the PerformanceEval.txt and .csv files. This is also used in combination with overall run time to determine the best combination of descriptor and detector.

### MP.9 Performance Evaluation 3
Each combination of detector/descriptor is timed and a comparison is made based on the number of keypoints and total elapsed time. Based on the number of matched keypoints and the elapsed time, the following combinations of Detector/Descriptor are in the top 3. Other  combinations took less time to run, but also resulted in much fewer matches. The selection of top 3 could vary based on the use-case and the desired number of keypoints. If a smaller number of keypoint matches is found to be sufficient for the desired use case, then a faster algorithm could be used.

Detector/Descriptor: FAST / BRIEF
* Total keypoints over all images: 4094
* Total matched keypoints over all images: 2831
* Total time for all images: 535.139 ms

Detector/Descriptor: FAST / ORB
* Total keypoints over all images: 4094
* Total matched keypoints over all images: 2768
* Total time for all images: 581.888 ms

Detector/Descriptor: FAST / FREAK
* Total keypoints over all images: 4094
* Total matched keypoints over all images: 2233
* Total time for all images: 1145.69 ms