# Print-N-bit-binary-numbers-having-more-1-s-than-0-s-in-all-prefixes-in-CPP

N-bit binary numbers having more  or equal 1’s than 0’s in C++
Here, in this page we will discuss the program to print n-bit binary numbers having more or equal 1’s than 0’s in C++ programming language. We are given with an integer value n, and need to print n-bit binary numbers.

Example :

Input : 4
Output : 1111 1110 1101 1100 1011 1010
Explanation : 1111 1110 1101 1100 1011 1010 all these are the requied binary numbers.
N-bit binary numbers having more 1’s than 0’s in C++
Method 1 (Recursive Approach) :
In this method we use recursion. At each point in the recursion, we append 0 and 1 to the partially formed number and recur with one less digit. 

Create a recursive function say printRec(string number, int extraOnes, int remainingPlaces)
Check if(remainingPlaces==0), if it is true then print number and return //Base condition
Otherwise call the function by appending “1” i.e, printRec(number + “1”, extraOnes+1, remainingPlaces-1)
Now, check if(extraOnes > 0),then call printRec(number+ “0”, extraOnes-1, remainingPlaces-1), i.e, by appending 0 to number.
Code to find N-bit binary numbers having more or equal 1's than 0's in C++
Run

#include <bits/stdc++.h>
using namespace std;

//Recursive function to print the required numbers
void printRec(string number, int extraOnes,int remainingPlaces)
{
   if (remainingPlaces==0) {
     cout << number << " "; return; 
   } 
   printRec(number + "1", extraOnes + 1, remainingPlaces - 1);

   if (extraOnes > 0) 
   printRec(number + "0", extraOnes - 1,remainingPlaces - 1);
}

// Driver code
int main()
{
   int n = 4;

   string str = ""; 
   printRec(str, 0, n);

   return 0;
}
Output :


1111 1110 1101 1100 1011 1010
Method 2 (Using Non-Recursive Approach) :
In this method we will discuss the non-recursive approach to print the required binary numbers having more or equal1’s than 0’s.

Declare two variables say first and last.
Set first to 1<<(N-1) and last to first*2.
Run a loop over the range i.e, last-1 to first.
Now, quire only those which satisfies the condition (i.e, one_cnt >= zero_cnt)
N-bit binary numbers
Code in C++
Run
#include <bits/stdc++.h>
using namespace std;

// Function to get the binary representation
// of the number N
string getBinaryRep(int N, int num_of_bits)
{
   string s = "";
   num_of_bits--;

   // loop for each bit
   while (num_of_bits >= 0)
   {
     if (N & (1 << num_of_bits))
        s.append("1");
     else
        s.append("0");
     num_of_bits--;
    }
    return s;
}


vector <string> NBitBinary(int N)
{
   vector <string> s;
   int first = 1 << (N - 1);
   int last = first * 2;

   for (int i = last - 1; i >= first; --i)
   {
      int zero_cnt = 0;
      int one_cnt = 0;
      int t = i;
      int num_of_bits = 0;

      // longest prefix check
      while (t)
      {
         if (t & 1)
           one_cnt++;
         else
           zero_cnt++;
         num_of_bits++;
         t = t >> 1;
      }

      if (one_cnt >= zero_cnt)
      {
          // do sub-prefixes check
          bool all_prefix_match = true;
          int msk = (1 << num_of_bits) - 2;
          int prefix_shift = 1;
          while (msk)
          {

               int prefix = (msk & i) >> prefix_shift;
               int prefix_one_cnt = 0;
               int prefix_zero_cnt = 0;
               while (prefix)
               {
                 if (prefix & 1)
                     prefix_one_cnt++;
                 else
                     prefix_zero_cnt++;
                 prefix = prefix >> 1;
               }
               if (prefix_zero_cnt > prefix_one_cnt)
               {
                  all_prefix_match = false;
                  break;
               }
               prefix_shift++;
               msk = msk & (msk << 1);
          }
          if (all_prefix_match)
          {
               s.push_back(getBinaryRep(i, num_of_bits));
          }
      }
   }
   return s;
}

// Driver code
int main()
{
      int n = 4;

      vector <string>results = NBitBinary(n);
      for (int i = 0; i < results.size(); ++i)
           cout << results[i] << " ";

      return 0;
}
Output :


1111 1110 1101 1100 1011 1010
