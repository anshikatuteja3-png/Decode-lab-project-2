#include <iostream>
#include <vector>
#include <cmath>
#include <algorithm>
#include <cstdlib>
#include <ctime>

using namespace std;

struct DataPoint {
    double x1, x2;
    int label;
};

// Calculate Euclidean Distance
double distance(DataPoint a, DataPoint b) {
    return sqrt(pow(a.x1 - b.x1, 2) + pow(a.x2 - b.x2, 2));
}

// KNN Classifier
int classify(vector<DataPoint>& trainData, DataPoint testPoint, int k) {
    vector<pair<double, int>> distances;

    for (auto point : trainData) {
        distances.push_back({distance(point, testPoint), point.label});
    }

    sort(distances.begin(), distances.end());

    int count0 = 0, count1 = 0;

    for (int i = 0; i < k; i++) {
        if (distances[i].second == 0)
            count0++;
        else
            count1++;
    }

    return (count1 > count0) ? 1 : 0;
}

int main() {

    srand(time(0));

    // Dataset
    vector<DataPoint> dataset = {
        {1,2,0},{2,1,0},{2,2,0},{3,2,0},{3,3,0},
        {6,5,1},{7,6,1},{8,6,1},{8,7,1},{9,8,1}
    };

    random_shuffle(dataset.begin(), dataset.end());

    vector<DataPoint> trainData, testData;

    // 80% Training 20% Testing
    for(int i=0;i<dataset.size();i++){
        if(i<8)
            trainData.push_back(dataset[i]);
        else
            testData.push_back(dataset[i]);
    }

    int correct = 0;
    int k = 3;

    cout << "----- Test Results -----\n";

    for(auto point : testData){

        int prediction = classify(trainData, point, k);

        cout << "Actual: " << point.label
             << "  Predicted: " << prediction << endl;

        if(prediction == point.label)
            correct++;
    }

    double accuracy = (double)correct / testData.size() * 100;

    cout << "\nTraining Samples : " << trainData.size() << endl;
    cout << "Testing Samples  : " << testData.size() << endl;
    cout << "Model Accuracy   : " << accuracy << "%" << endl;

    return 0;
}
