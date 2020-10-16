# Minimum-Swapping
There are N couples sitting in a row of length 2 * N. They are currently ordered randomly, but would like to rearrange themselves so that each couple's partners can sit side by side.  What is the minimum number of swaps necessary for this to happen?


#include<bits/stdc++.h> 
using namespace std; 

//includes function of a and b
void updateindex(int index[], int a, int ai, int b, int bi) 
{ 
	index[a] = ai; 
	index[b] = bi; 
} 

//function requires minimum swaps to arrange all the elements
int minSwapsUtil(int arr[], int pairs[], int index[], int i, int n) 
{ 
	//if all are paired then return zero 
	if (i > n) return 0; 

	//if pair is valid then move ahead 
	if (pairs[arr[i]] == arr[i+1]) 
		return minSwapsUtil(arr, pairs, index, i+2, n); 

	//if arr[i] does not pair with arr[i+1] then move recursively 
	int one = arr[i+1]; 
	int indextwo = i+1; 
	int indexone = index[pairs[arr[i]]]; 
	int two = arr[index[pairs[arr[i]]]]; 
	swap(arr[i+1], arr[indexone]); 
	updateindex(index, one, indexone, two, indextwo); 
	int a = minSwapsUtil(arr, pairs, index, i+2, n); 

	//restore the other back indices
	swap(arr[i+1], arr[indexone]); 
	updateindex(index, one, indextwo, two, indexone); 
	one = arr[i], indexone = index[pairs[arr[i+1]]]; 

	//swap arr[i] with arr[i+1] for minimum swaps
	two = arr[index[pairs[arr[i+1]]]], indextwo = i; 
	swap(arr[i], arr[indexone]); 
	updateindex(index, one, indexone, two, indextwo); 
	int b = minSwapsUtil(arr, pairs, index, i+2, n); 

	//return previous indices
	swap(arr[i], arr[indexone]); 
	updateindex(index, one, indextwo, two, indexone); 

	// Return minimum of two cases 
	return 1 + min(a, b); 
} 

// Returns minimum swaps required 
int minSwaps(int n, int pairs[], int arr[]) 
{ 
	int index[2*n + 1]; // To store indices of array elements 

	// Store index of each element in array index 
	for (int i = 1; i <= 2*n; i++) 
		index[arr[i]] = i; 

	// Call the recursive function 
	return minSwapsUtil(arr, pairs, index, 1, 2*n); 
} 

// Driver program 
int main() 
{ 
	
	int arr[] = {0, 3, 5, 6, 4, 1, 2}; 

	// if (a, b) is pair than we have assigned elements 
	// in array such that pairs[a] = b and pairs[b] = a 
	int pairs[] = {0, 3, 6, 1, 5, 4, 2}; 
	int m = sizeof(arr)/sizeof(arr[0]); 

	int n = m/2; // Number of pairs n is half of total elements 

	//if no elements are there consider n pair
	cout << "Min swaps required is " << minSwaps(n, pairs, arr); 
	return 0; 
} 
