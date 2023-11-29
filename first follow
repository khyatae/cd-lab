#include <iostream>
#include <vector>
#include <map>
#include <set>
using namespace std;

void find_first(char non_terminal, vector<string>& grammar, map<char, set<char>>& first_arr)
{
    if (first_arr.find(non_terminal) != first_arr.end())
    {
        return;
    }

    set<char> first_set;

    for (string production : grammar)
    {
        if (production[0] == non_terminal)
        {
            int j = 2;
            while (j < production.size())
            {
                if (production[j] == '@')
                {
                    first_set.insert('@');
                    break;
                }
                else if (production[j] >= 'A' && production[j] <= 'Z')
                {
                    find_first(production[j], grammar, first_arr);
                    first_set.insert(first_arr[production[j]].begin(), first_arr[production[j]].end());
                    first_set.erase('@');
                    if (first_arr[production[j]].find('@') == first_arr[production[j]].end())
                    {
                        break;
                    }
                }
                else
                {
                    first_set.insert(production[j]);
                    break;
                }
                ++j;
            }
            if(j==production.size())
            {
                first_set.insert('@');
            }
        }
    }

    first_arr[non_terminal] = first_set;
}

void compute_first(vector<string> grammar, map<char, set<char>>& first_arr)
{
    for (string production : grammar)
    {
        find_first(production[0], grammar, first_arr);
    }
}

void find_follow(char non_terminal, vector<string>& grammar, map<char, set<char>>& follow_arr,map<char, set<char>>& first_arr)
{
    if (follow_arr.find(non_terminal) != follow_arr.end())
    {
        return;
    }

    set<char> follow_set;
    if(non_terminal==grammar[0][0])
    {
     follow_set.insert('$');
    }

    for (string production : grammar)
    {
        for(int pos=2;pos<production.size();pos++)
        {
        char st=production[pos];
        if (st == non_terminal)
        {
            int j = pos+1;
            while (j < production.size())
            {

                if (production[j] >= 'A' && production[j] <= 'Z')
                {

                    follow_set.insert(first_arr[production[j]].begin(), first_arr[production[j]].end());
                    if (first_arr[production[j]].find('@') == first_arr[production[j]].end())
                    {
                        break;
                    }
                }
                else
                {
                    follow_set.insert(production[j]);
                    break;
                }
                ++j;
            }
            if(j==production.size() && production[0]!=non_terminal)
            {
                find_follow(production[0],grammar,follow_arr,first_arr);
                follow_set.insert(follow_arr[production[0]].begin(), follow_arr[production[0]].end());
            }
        }
    }
    }
    follow_set.erase('@');
    follow_arr[non_terminal] = follow_set;
}

void compute_follow(vector<string> grammar, map<char, set<char>>& follow_arr,map<char, set<char>>& first_arr)
{
    for (string production : grammar)
    {
        for(char non_terminal:production)
        {
         if(non_terminal>='A' && non_terminal<='Z')
         {
         find_follow(non_terminal, grammar, follow_arr,first_arr);
         }
        }
    }
}

int main()
{
    int n;
    cout << "Enter the number of productions: ";
    cin >> n;

    vector<string> grammar(n, "");
    cout << "Enter the productions:\n";
    for (int i = 0; i < n; i++)
    {
        cin >> grammar[i];
    }

    map<char, set<char>> first_arr;
    compute_first(grammar, first_arr);

    for (auto x : first_arr)
    {
        cout << "FIRST(" << x.first << ") = {";
        for (char terminal : x.second)
        {
            cout << terminal << ",";
        }
        cout << "}\n";
    }


    map<char, set<char>> follow_arr;
    compute_follow(grammar, follow_arr,first_arr);

    for (auto x : follow_arr)
    {
        cout << "FOLLOW(" << x.first << ") = {";
        for (char terminal : x.second)
        {
            cout << terminal << ",";
        }
        cout << "}\n";
    }


    return 0;
}
