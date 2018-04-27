# C++Learning
//An experiment about designing a  barbershop system
//
<Customer.h>
#pragma once
#include<iostream>
#include<string>
#include<queue>
using namespace std;
static double fee;
//Customer base class
class Customer
{
protected:
	string name;			//姓名
	double card_number;		//卡号
	double balance;			//余额
	double discount;		//折扣
public:
	Customer() {}
	~Customer() {}
	void SetInformation(string na, double cn);		//输入会员信息√
	void Get_balance();		//获取余额√
	int Retrieve(double);	//检索卡号√
	int hair_cut();			//理发√
	int perm();				//烫发√
	int hair_care();		//护理√

};


<VariousCard.h>
#pragma once
#include "customer.h"
#include<iostream>
using namespace std;
class Bronze;
class Silver;
class Gold;
//Manager class
class Manager
{
	double Login;			//账号
	double password;		//密码
public:
	Manager()
	{
		Login = 100000;			//管理员账号
		password = 66668888;		//管理员密码
	}
	~Manager() {}
	void List(Bronze &p);		//会员列表
	void List(Silver &p);
	void List(Gold &p);
	void Get_fee();				//收费统计
	int Register(double lg, double pw);		//登录检验
};
//Bronze VIP card derived class
class Bronze :
	public Customer
{
public:
	Bronze() { discount = 0.95; }
	~Bronze() {}
	void SetInformation(string na, double cn);
	void Top_up();			//充值√
	friend void Manager::List(Bronze &);
	friend ostream& operator<<(ostream& out, Bronze &ob1)
	{
		out << ob1.name << '\t' << ob1.card_number << '\t' << "青铜会员" << endl;
		return out;
	}
};
//Silver VIP card derived class
class Silver :
	public Customer
{
public:
	Silver() { discount = 0.9; }
	~Silver() {}
	void SetInformation(string na, double cn);
	void Top_up();			//充值√
	friend void Manager::List(Silver &);
	friend ostream& operator<<(ostream& out, Silver &ob1)
	{
		out << ob1.name << '\t' << ob1.card_number << '\t' << "白银会员" << endl;
		return out;
	}
};
//Gold VIP card class
class Gold :
	public Customer
{
public:
	Gold() { discount = 0.8; }
	~Gold() {}
	void SetInformation(string na, double cn);
	void Top_up();			//充值√
	friend void Manager::List(Gold &);
	friend ostream& operator<<(ostream& out, Gold &ob1)
	{
		out << ob1.name << '\t' << ob1.card_number << '\t' << "黄金会员" << endl;
		return out;
	}
};
  
  
<Customer.cpp>
#include "Customer.h"
#include<iostream>
#include<string>
using namespace std;

/*Customer类的成员函数*/
void Customer::SetInformation(string na, double cn)		//输入会员信息
{
	name = na;
	card_number = cn;
	cout << "您的卡号为：" << card_number << endl;
}
void Customer::Get_balance()		//余额查询
{
	cout << "卡上余额：" << balance << "元" << endl;
}
int Customer::Retrieve(double card)		//检索卡号
{
	if (card == card_number)
	{
		cout << name << "，欢迎光临本店!" << endl;
		return 1;			//卡号匹配成功，返回1
	}
	return 0;				//卡号匹配失败，返回0
}
int Customer::hair_cut()			//理发
{
	int index;
	cout << "理发一次为" << 30 * discount << "元" << endl;
	cout << "请确认是否进行消费" << endl;
	cout << "YES(1)" << '\t' << "NO(0)" << endl;
	cin >> index;
	if (index == 1)			//确认交易
	{
		if (balance<30 * discount)			//余额不足时，自动取消消费
		{
			cout << "对不起，您的余额不足，请充值" << endl;
			return 1;
		}
		cout << "交易成功！" << endl;
		balance -= 30 * discount;			//余额足够，交易结束，余额扣除30元
		Get_balance();
		return 0;
	}
	else				//取消交易
	{
		cout << "交易已取消" << endl;
		return 1;
	}
}
int Customer::perm()			//烫发
{
	int index;
	cout << "烫发一次为" << 200 * discount << "元" << endl;
	cout << "请确认是否进行消费" << endl;
	cout << "YES(1)" << '\t' << "NO(0)" << endl;
	cin >> index;
	if (index == 1)			//确认交易
	{
		if (balance<200 * discount)			//余额不足时，自动取消消费
		{
			cout << "对不起，您的余额不足，请充值" << endl;
			return 1;
		}
		cout << "交易成功！" << endl;
		balance -= 200 * discount;			//余额足够，交易结束，余额扣除200元
		Get_balance();
		return 0;
	}
	else				//取消交易
	{
		cout << "交易已取消" << endl;
		return 1;
	}
}
int Customer::hair_care()			//护理
{
	int index;
	cout << "护理一次为" << 50 * discount << "元" << endl;
	cout << "请确认是否进行消费" << endl;
	cout << "YES(1)" << '\t' << "NO(0)" << endl;
	cin >> index;
	if (index == 1)			//确认交易
	{
		if (balance<50 * discount)			//余额不足时，自动取消消费
		{
			cout << "对不起，您的余额不足，请充值" << endl;
			return 1;
		}
		cout << "交易成功！" << endl;
		balance -= 50 * discount;			//余额足够，交易结束，余额扣除50元
		Get_balance();
		return 0;
	}
	else				//取消交易
	{
		cout << "交易已取消" << endl;
		return 1;
	}
}


<VariousCard.cpp>
#include"Customer.h"
#include "VariousCard.h"
#include<iostream>
#include<string>
using namespace std;
/*Manager类的成员函数*/
int Manager::Register(double lg, double pw)
{
	if (lg == Login)
		if (pw == password)
		{
			cout << "登录成功！" << endl;
			return 1;
		}
	return 0;
}
void Manager::Get_fee()
{
	cout << "收费总额：" << ::fee << endl;
}
void Manager::List(Bronze &p)
{
	cout << p;
}
void Manager::List(Silver &p)
{
	cout << p;
}
void Manager::List(Gold &p)
{
	cout << p;
}
/*青铜会员类成员函数*/
void Bronze::SetInformation(string na, double cn)
{
	name = na;
	card_number = cn;
	balance = 500;			//办理青铜会员需要一次充值500元
	fee += 500;
	cout << "您的卡号为：" << card_number << endl;
}
void Bronze::Top_up()
{
	balance += 300;
	::fee += 300;
	cout << "充值成功！卡上余额为" << balance << endl;
}


/*白银会员类成员函数*/
void Silver::SetInformation(string na, double cn)
{
	name = na;
	card_number = cn;
	balance = 1000;			//办理白银会员需要一次充值1000元
	fee += 1000;			//记录收费
	cout << "您的卡号为：" << card_number << endl;
}
void Silver::Top_up()		//充值
{
	balance += 600;
	fee += 600;
	cout << "充值成功！卡上余额为" << balance << endl;
}

/*黄金会员类成员函数*/
void Gold::SetInformation(string na, double cn)
{
	name = na;
	card_number = cn;
	balance = 2000;			//办理黄金会员需要一次充值2000元
	fee += 2000;			//记录收费
	cout << "您的卡号为：" << card_number << endl;
}
void Gold::Top_up()
{
	balance += 1200;
	fee += 1200;
	cout << "充值成功！卡上余额为" << balance << endl;
}


<main.cpp>
#include"Customer.h"
#include"VariousCard.h"
#include<iostream>
#include<string>
#include<queue>
#include<fstream>
using namespace std;
void menu();		//生成菜单界面
void menu1();		//办理会员卡下属菜单
void menu2();		//会员列表下属菜单
void menu3();		//理发师选择表
void operate_queue();		//队列操作
void service();		//菜单服务
int number1 = 0;		//青铜会员计数
int number2 = 0;		//白银会员计数
int number3 = 0;		//黄金会员计数
queue<int>Barber;		//理发师队列
const int Filesize = 30;
const string Sentence1 = "办理青铜会员： ";
const string Sentence2 = "办理白银会员： "; 
const string Sentence3 = "办理黄金会员： ";
const string Sentence4 = "青铜会员充值： ";
const string Sentence5 = "白银会员充值： ";
const string Sentence6 = "黄金会员充值： ";

int main()
{
	try
	{
		service();
	}
	catch (...)
	{
		cout << endl << "对不起，您输入的操作指令有误" << endl;
		service();
	}
	return 0;
}

void menu()			//生成菜单界面
{
	cout << '\t' << "| ——美发服务应用系统—— |" << endl;
	cout << '\t' << "| ***** 1.办理会员卡 ***** |" << endl;
	cout << '\t' << "| ******** 2.理发 ******** |" << endl;
	cout << '\t' << "| ******** 3.烫发 ******** |" << endl;
	cout << '\t' << "| ******** 4.护理 ******** |" << endl;
	cout << '\t' << "| ***** 5.会员卡充值 ***** |" << endl;
	cout << '\t' << "| ****** 6.会员列表 ****** |" << endl;
	cout << '\t' << "| ****** 7.收费统计 ****** |" << endl;
	cout << '\t' << "| ******** 8.退出 ******** |" << endl;
	cout << "请输入您需要的服务内容：";
}
void menu1()		//办理会员卡下属菜单
{
	cout << '\t' << "| —— 会员卡类型列表 —— |" << endl;
	cout << '\t' << "| ****** 1.青铜会员 ****** |" << endl;
	cout << '\t' << "| ****** 2.白银会员 ****** |" << endl;
	cout << '\t' << "| ****** 3.黄金会员 ****** |" << endl;
	cout << "请输入您要办理的会员卡类型：";
}
void menu2()		//会员列表下属菜单
{
	cout << '\t' << "| ——  本期会员列表查询  —— |" << endl;
	cout << '\t' << "| *******  1.青铜会员  ******* |" << endl;
	cout << '\t' << "| *******  2.白银会员  ******* |" << endl;
	cout << '\t' << "| *******  3.黄金会员  ******* |" << endl;
	cout << '\t' << "| ****** 4.往期会员列表 ****** |" << endl;
	cout << '\t' << "| *********  5.退出  ********* |" << endl;
	cout << "请输入您要进行的操作：";
}
void service()
{
	ofstream fout("Customer Information.txt", ios::out | ios::app);		//定义输出文件流对象fout，打开输出文件Customer Information.txt
	if (!fout)
	{
		cout << "Can't open output file !" << endl;
		exit(1);
	}
	ofstream feeout("Total Charge.txt", ios::out | ios::app);
	if (!feeout)
	{
		cout << "Can't open output file !" << endl;
		exit(1);
	}
	ifstream fincn("Customer Information.txt", ios::in);
	if (!fincn)
	{
		cout << "Can't open input file !" << endl;
		exit(1);
	}
	string n1[Filesize], n2[Filesize];
	double c[Filesize];
	int n=0;
	while(fincn)				//扫描文件，得到最大卡号
	{
		fincn >> n1[n] >> c[n] >> n2[n];
		n++;
	}
	char index;					//选项
	double cn = c[n - 2] + 1;			//初始卡号
	Bronze customer1[10];		//发放青铜会员卡10名
	Silver customer2[5];		//发放白银会员卡5名
	Gold customer3[3];			//发放黄金会员卡3名
	if(cn<0)
		cn = 1800;
	menu();
	cin >> index;
	if (index > '8' || index < '1')
	{
		throw index;
	}
	while (index != '8')
	{
		switch (index)
		{
		case '1':			//办理会员卡
		{
			char type;		//选择会员类型
			menu1();
			cin >> type;
			if (type < '1' || type>'3')
			{
				throw type;
			}
			switch (type)
			{
			case '1':
			{
				if (number1<10)
				{
					string na;
					cout << "请输入会员姓名：" << endl;
					cin >> na;
					customer1[number1].SetInformation(na, cn);
					fout << customer1[number1];
					number1++;
					cn++;
					feeout << endl << Sentence1 << 500;
					break;
				}
				else
				{
					cout << "对不起，青铜会员名额已满" << endl;
					break;
				}
			}
			case '2':
			{
				if (number1<5)
				{
					string na;
					cout << "请输入会员姓名：" << endl;
					cin >> na;
					customer2[number2].SetInformation(na, cn);
					fout << customer2[number2];
					number2++;
					cn++;
					feeout << endl << Sentence2 << 1000;
					break;
				}
				else
				{
					cout << "对不起，白银会员名额已满" << endl;
					break;
				}
			}
			case '3':
			{
				if (number3<3)
				{
					string na;
					cout << "请输入会员姓名：" << endl;
					cin >> na;
					customer3[number3].SetInformation(na, cn);
					fout << customer3[number3];
					number3++;
					cn++;
					feeout << endl << Sentence3 << 2000;
					break;
				}
				else
				{
					cout << "对不起，黄金会员名额已满" << endl;
					break;
				}
			}
			}
			break;
		}
		case '2':			//理发
		{
			double card;
			int j = 0, k = 0, l = 0;
			cout << "请输入您的卡号：" << endl;
			cin >> card;
			for (j; j<10; j++)		//青铜卡号检索
			{
				if (customer1[j].Retrieve(card))
				{
					if (!customer1[j].hair_cut())
					{
						operate_queue();
					}
					break;
				}
			}
			for (k; k<5; k++)
			{
				if (customer2[k].Retrieve(card))
				{
					if (!customer2[k].hair_cut())
					{
						operate_queue();
					}
					break;
				}
			}
			for (l; l<3; l++)
			{
				if (customer3[l].Retrieve(card))
				{
					if (!customer3[l].hair_cut())
					{
						operate_queue();
					}
					break;
				}
			}
			if (j >= 10 && k >= 5 && l >= 3)
				cout << "对不起，无对应会员卡号,交易失败。" << endl;
			break;
		}
		case '3':			//烫发
		{
			double card;
			int j = 0, k = 0, l = 0;
			cout << "请输入您的卡号：" << endl;
			cin >> card;
			for (j; j<10; j++)		//青铜卡号检索
			{
				if (customer1[j].Retrieve(card))
				{
					if (!customer1[j].perm())
					{
						operate_queue();
					}
					break;
				}
			}
			for (k; k<5; k++)
			{
				if (customer2[k].Retrieve(card))
				{
					if (!customer2[k].perm())
					{
						operate_queue();
					}
					break;
				}
			}
			for (l; l<3; l++)
			{
				if (customer3[l].Retrieve(card))
				{
					if (!customer3[l].perm())
					{
						operate_queue();
					}
					break;
				}
			}
			if (j >= 10 && k >= 5 && l >= 3)
				cout << "对不起，无对应会员卡号,交易失败。" << endl;
			break;
		}
		case '4':			//护理
		{
			double card;
			int j = 0, k = 0, l = 0;
			cout << "请输入您的卡号：" << endl;
			cin >> card;
			for (j; j<10; j++)		//青铜卡号检索
			{
				if (customer1[j].Retrieve(card))
				{
					if (!customer1[j].hair_care())
					{
						operate_queue();
					}
					break;
				}
			}
			for (k; k<5; k++)
			{
				if (customer2[k].Retrieve(card))
				{
					if (!customer2[k].hair_care())
					{
						operate_queue();
					}
					break;
				}
			}
			for (l; l<3; l++)
			{
				if (customer3[l].Retrieve(card))
				{
					if (!customer3[l].hair_care())
					{
						operate_queue();
					}
					break;
				}
			}
			if (j >= 10 && k >= 5 && l >= 3)
				cout << "对不起，无对应会员卡号,交易失败。" << endl;
			break;
		}
		case '5':			//充值
		{
			double card;
			int j = 0, k = 0, l = 0;
			cout << "请输入您的卡号：" << endl;
			cin >> card;
			for (j; j<10; j++)		//青铜卡号检索
			{
				if (customer1[j].Retrieve(card))
				{
					customer1[j].Top_up();
					feeout << endl << Sentence4 << 300;
					break;
				}
			}
			for (k; k<5; k++)
			{
				if (customer2[k].Retrieve(card))
				{
					customer2[k].Top_up();
					feeout << endl << Sentence5 << 600;
					break;
				}
			}
			for (l; l<3; l++)
			{
				if (customer3[l].Retrieve(card))
				{
					customer3[l].Top_up();
					feeout << endl << Sentence6 << 1200;
					break;
				}
			}
			if (j >= 10 && k >= 5 && l >= 3)
				cout << "对不起，无对应会员卡号,交易失败。" << endl;
			break;
		}
		case '6':			//会员列表，管理员使用
		{
			double lg;
			double pw;
			Manager boss;
			cout << "请输入管理员账号：" << endl;
			cin >> lg;
			cout << "请输入密码：" << endl;
			cin >> pw;
			if (boss.Register(lg, pw))
			{
				menu2();
				char option;
				cin >> option;
				if (option < '1' || option>'5')
				{
					throw option;
				}
				while (option<'5')			//查询会员列表
				{
					switch (option)
					{
					case '1':			//青铜会员
					{
						cout << "————会员列表————" << endl;
						cout << "姓名\t" << "卡号\t" << "会员类型" << endl;
						for (int x = 0; x<number1; x++)
							boss.List(customer1[x]);
						break;
					}
					case '2':			//白银会员
					{
						cout << "————会员列表————" << endl;
						cout << "姓名\t" << "卡号\t" << "会员类型" << endl;
						for (int x = 0; x<number2; x++)
							boss.List(customer2[x]);
						break;
					}
					case '3':			//黄金会员
					{
						cout << "————会员列表————" << endl;
						cout << "姓名\t" << "卡号\t" << "会员类型" << endl;
						for (int x = 0; x<number3; x++)
							boss.List(customer3[x]);
						break;
					}
					case '4':			//从文本文件中读取往期的会员名单
					{
						string fname1[Filesize],fname2[Filesize];
						double fcn[Filesize];
						ifstream fin("Customer Information.txt", ios::in);
						if (!fin)
						{
							cout << "Can't open input file !" << endl;
							exit(1);
						}
						int num=0;
						while (fin)		//文件未读到尾
						{
							fin >> fname1[num] >> fcn[num] >> fname2[num];
							num++;
						}
						
						for(int i=0;i<num-1;i++)
						{
							cout << fname1[i] << '\t' << fcn[i] << '\t' << fname2[i] << endl;
						}
						fin.close();
					}
					}
					cout << endl;
					menu2();
					cin >> option;
				}
				break;
			}
			else
				cout << "账号不存在或密码错误！" << endl;
			break;
		}
		case '7':			//收费统计，管理员使用
		{
			double lg;
			double pw;
			Manager boss;
			cout << "请输入管理员账号：" << endl;
			cin >> lg;
			cout << "请输入密码：" << endl;
			cin >> pw;
			if (boss.Register(lg, pw))
			{
				boss.Get_fee();
				break;
			}
			else
				cout << "账号不存在或密码错误！" << endl;
			break;
		}
		case '8':
			feeout.close(); fout.close(); exit(1); break;
		default:
			cout << "对不起，您输入的指令有误" << endl; break;
		}
		cout << endl;
		menu();
		cin >> index;
	}
}
void operate_queue()
{
	if (Barber.empty())
		Barber.push(1);
	else
	{
		if (Barber.size()>1)
			cout << "请稍等，您前面还有" << Barber.size() - 1 << "人在排队" << endl;
		Barber.push(1);
	}
}
