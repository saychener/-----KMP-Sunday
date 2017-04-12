# -----KMP&&Sunday

/*--------------------------------*/
#include<iostream>
#include<vector>
#include<string>
using namespace std;

vector<int> compute_prefix(string s) {
	int len = s.size();
	vector<int> tmp = { 0};
	int k = 0;
	for (int i = 1;i < len;i++) {
		while (k>0&&s[k]!=s[i]) {
			k = tmp[k-1]; //注意下标是从0开始的
		}
		if (s[k] == s[i]) {
			k++;
		}
		tmp.push_back(k);
	}
	return tmp;
}
//返回父串中包含子串的个数
int KMP(string text,string sub) {
	int count = 0;
	int n = text.size();
	int m = sub.size();
	vector<int> tmp = compute_prefix(sub);
	int q = 0;//已经匹配的字符数
	for (int i = 0;i < n;i++) {
		while (q>0&&sub[q]!=text[i]) {
			q = tmp[q-1];         //字符数组下标均从0开始！！！   若已有q个字符匹配则从下一次字符开始比较，若相等则进入下面的if执行q++，
			                      //若不等则应跳过若干个字符，这里应从之前保存的prefix中寻找对应的最长前缀
		}
		if (sub[q] == text[i]) {
			q++;
		}
		if (q == m) {
			count++;
			q = tmp[q-1];  //注意这里的前缀数组是从下标0开始的
		}
	}
	return count;
}


int Sunday(char *text, char *substring) {
	int len1 = strlen(text);
	int len2 = strlen(substring);

	int charStep[256];    //ASCII码表中一共有256个字符
	for (int i = 0; i < 256; ++i)
		charStep[i] = -1;
	for (int i = 0; i < len2; ++i)
		charStep[(int)substring[i]] = i;   //记录子串中的每个字符的先后顺序

	for (int i = 0; i <= len1 - len2;)
	{
		int j = 0;
		while (j < len2) {
			if (text[i] == substring[j]) {
				++i;
				++j;
			}
			else {
				char* p = text + i + len2 - j;
				if (charStep[(int)*p] == -1) {
					i = p - text + 1;
				}
				else {
					i = p - charStep[(int)*p] - text;
				}
				break;
			}
		}

		if (j == len2) {
			return i - len2;          //返回匹配的位置
		}
	}

	return -1;
}

int main() {
	char *text = "codemonkey";
	char *sub = "nke";
	//string text = "bacbababaabcbab";
	//string sub = "aba";
	cout<<Sunday(text,sub);
	//cout << KMP(text,sub) << endl;
	system("pause");
	return 0;
}
