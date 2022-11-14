# My first Project
#include <iostream>
#include <vector>
#include <string>
#include <sstream>
using namespace std;
struct app
{
    int p;
    int q;
    int m;
    vector<int> periods;
};

vector<string> getting_intputs(){
    string buffer;
    vector<string> inputs;
    while(getline(cin , buffer , '\n')){
        inputs.push_back(buffer);
    }
    inputs.erase(inputs.begin());
    return inputs;
}
vector<vector<string>> spilit(vector<string> inputs){
    vector<vector<string>> buffer(inputs.size());
    for(int i = 0 ; i < inputs.size() ; i++){
        stringstream ss(inputs[i]);
        string s;
        while(getline(ss , s , ' ')){
            buffer[i].push_back(s);
        }
    }
    return buffer;
}
void initilizing_apps(vector<vector<string>>const str , vector<app> &apps){
    for(int i = 0 ; i < str.size() ; i++){
        apps[i].p = stoi(str[i][0]);
        apps[i].q = stoi(str[i][1]);
        apps[i].m = stoi(str[i][2]);
        for(int j = 3 ; j < str[i].size() ; j++){
            apps[i].periods.push_back(stoi(str[i][j]));
        }
    }
}

bool conditions(int p , int q , int time1 , int time2){
    if(p < time1 - time2 || q < time1 - time2){
        return true;
    }
    return false;
}

vector<bool> check_maximum_duration(vector<app> const apps){
    vector<bool> result;
    int flag;
    for(int i = 0 ; i < apps.size() ; i++){
        flag = 0;
        for(int j = 0 ; j < apps[i].periods.size() ; j = j+2){
            if(conditions(apps[i].p , apps[i].q , apps[i].periods[j+1] , apps[i].periods[j])){
                result.push_back(false);
                break;
            }
            flag ++;
        }
        if(flag == apps[i].periods.size()/2){
            result.push_back(true);
        }
    }
    return result;
}

void outputs(vector<bool>const result){
    for(auto it = result.begin() ; it != result.end() ; ++it){
        cout << int(*it);
    }
    cout << endl;
}


int main(){
    int number_of_apps;
    cin >> number_of_apps;
    vector<app>  apps(number_of_apps);
    vector<vector<string>> inputs = spilit(getting_intputs());
    initilizing_apps(inputs , apps);
    outputs(check_maximum_duration(apps));
    return 0;
}
