#include <iostream.h>
#include <conio.h>
#include <math.h>
#include <iomanip.h>

void main()
{
	float a[100][100], s[100][100], c[100], b[100], t[100][100], d, x[100];
	int m,n,k,l;
	cout<<"Input banyaknya variabel : "; cin>>n;
	cout<<"Input banyaknya kendala  : "; cin>>m;
	cout<<endl;
	cout<<"Input koefisien fungsi tujuan : \n";
	for(int i=1; i<=n; i++)
	{
		cout<<"x"<<i<<" = "; cin>>c[i];
	}
	for(int i=1; i<=m; i++)
	{
		cout<<"\nInput Koefisien Kendala "<<i<<":\n";
		for(int j=1; j<=n; j++)
		{
			cout<<"x"<<j<<" = "; cin>>a[i][j];
		}
		cout<<"\nS"<<i<<" = "; cin>>s[i][1];
		cout<<"\nInput ruas kanan  = "; cin>>b[i];
	}
	cout<<endl;

   for(int k=1; k<=50; k++) cout<<"-";
	for(int i=1; i<=m+1; i++)
	{
		if(i==1)
		{
			cout<<"\nFungsi Tujuan :\n";
         cout<<"Z = ";
			int j=1;
			cout<<c[j]<<"x"<<j;
			for(int j=2; j<=n; j++)
			{
				if(c[j]>=0)
				{
					cout<<" + "<<c[j]<<"x"<<j;
				}
				else
				{
					cout<<" "<<c[j];
				}
			}
			cout<<endl;
			cout<<"\nDengan Syarat :\n";
		}
		else if(i>1)
		{
			int j=1;
			{
				cout<<a[i-1][j]<<"x"<<j;
			}
			for(int j=2; j<=n; j++)
			{
				if(a[i-1][j]>=0)
				{
					cout<<" + ";
					cout<<a[i-1][j]<<"x"<<j;
				}
				else
				{
					cout<<" ";
					cout<<a[i-1][j]<<"x"<<j;
				}
			}
			if(s[i-1][j]>=0)
			{
				cout<<" + "<<s[i-1][j]<<"S"<<(i-1)<<" = "<<b[i-1]<<endl;
			}
			else if(s[i-1][j]<0)
			{
				cout<<" "<<s[i-1][j]<<"S"<<(i-1)<<" = "<<b[i-1]<<endl;
			}
		}
	}
	cout<<endl;

	//MATRIKS AWAL
	for(int k=1; k<=50; k++) cout<<"-"; cout<<endl;
	cout<<setw(30)<<"TABEL SIMPLEKS AWAL\n";
	for(int k=1; k<=50; k++) cout<<"-"; cout<<endl;
	for(int i=1; i<=m+1; i++)
	{
		for(int j=1; j<=m+n+2; j++)
		{
			if(i==1)
			{
				t[1][1] = 1;
				t[i][j] = (-1*c[j-1]);
         }
         else if(i>1)
			{
				t[i][1]=0;
				if(j>1&&j<=n+1) t[i][j]=a[i-1][j-1];
				else if(j==(n+i))
				{
					t[i][j]=s[i-1][1];
				}
				else if(j>m+n+1&&i!=1)
				{
					t[i][j]=b[i-1];
				}
			}
		}
	}
	for(int i=1; i<=m+1; i++)
	{
		for(int j=1; j<=m+n+2; j++)
		{
			cout<<setw(6)<<t[i][j]<<setw(6);
		}
		cout<<endl;
	}

	//ITERASI
   int i;
   i=1;
   ulang:
   {
		float xmin=99999;
		for(int k=1; k<=50; k++) cout<<"-"; cout<<endl;
   	cout<<"Iterasi-"<<i<<endl;
      for(int k=1; k<=50; k++) cout<<"-"; cout<<endl;
      //NILAI TERKECIL BARIS PERTAMA
   	for(int r=1; r<=m+1; r++)
   	{
   		if (r==1)
   		{
   			for(int j=1; j<=m+n+2; j++)
      		{
      			if(t[r][j]<xmin&&t[r][j]!=0)
         		{
         			xmin = t[r][j];
            		k=j;
         		}
      		}
      		t[r][m+n+3]=0;
   		}
   		else if(r>1)
   		{
	   		if(t[r][k]!=0)
   			t[r][m+n+3]=t[r][m+n+2]/t[r][k];
            else if(t[r][k]==0)
            t[r][m+n+3]=0;
   		}
   	}
   	for(int r=1; r<=m+1; r++)
   	{
   		for(int j=1; j<=m+n+3; j++)
   		{
   			cout<<setw(6)<<t[r][j]<<setw(6)<<setprecision(2);
   		}
      	cout<<endl;
   	}
   	cout<<endl;
		//RASIO TERKECIL
		float xmin1=99999;
   	for(int r=2; r<=m+1; r++)
   	{
   		if(t[r][m+n+3]<xmin1&&t[r][m+n+3]>0)
			{
				xmin1=t[r][m+n+3];
				l=r;
			}
		}
		float q=t[l][k];
   	for(int r=1; r<=m+1; r++)
   	{
   		d=t[r][k]/t[l][k];
         if(r==l)
         {
          	for(int j=2; j<=m+n+2; j++)
          	{
					t[r][j]=t[r][j]/q;
          	}
         }
   		if(r!=l)
   		{
   			for(int j=2; j<=m+n+2; j++)
      		{
      			t[r][j]=t[r][j]-t[l][j]*d;
     	 		}
      	}
      	for(int j=1; j<=m+n+2; j++)
   		{
   			cout<<setw(6)<<t[r][j]<<setw(6)<<setprecision(3);
   		}
      	cout<<endl;
   	}
      i++;
      for(int j=2; j<=m+n+2; j++)
      {
      	if (t[1][j]<0) goto ulang;
      }
	}
   for(int k=1; k<=50; k++) cout<<"-"; cout<<endl;
   cout<<"\nDiperoleh :\n";
   for(int i=2; i<=m+1; i++)
   {
   	for(int j=2; j<=n+1; j++)
      {
         if(j==2)
         {
         	cout<<t[i][j]<<"x"<<(j-1);
         }
         else
         {
            cout<<" + ";
         	cout<<t[i][j]<<"x"<<(j-1);
         }
      }
      cout<<" = "<<t[i][m+n+2];
      cout<<endl;
   }
   //memunculkan nilai x
   int h, g, o, f;
   for(int j=2;j<=n+1;j++)
   {
      h=0,g=0;
   	for(int i=1;i<=m+1;i++)
      {
      	if(t[i][j]==1)
         {
         	h=h+1;
            o=i;
         }
         else if(t[i][j]>0&&t[i][j]!=1)
         {
          	g=g+1;
         }
      }
      if(h==1&&g==0)
      {
      	cout<<"\nx"<<(j-1)<<" = "<<t[o][m+n+2];
      }
      else if(g>0)
      {
      	cout<<"\nx"<<(j-1)<<" = "<<0;
      }

   }
   cout<<"\nZ = "<<t[1][m+n+2]<<endl;
   for(int k=1; k<=50; k++) cout<<"-"; cout<<endl;
	getch();
}