# Football-Standings
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>

#define BOYUT 30

//random sayı üret
int func2(int i)
{
	if (i < 10)
	{
		return rand() % 6;
	}

	if (i > 9 || i < 20)
	{
		return rand() % 5;
	}

	if (i > 19)
	{
		return rand() % 4;
	}
}

//Puan durumundaki istatiskleri tutmak için dizigenel matrisi olusturlmuş ve takımlar puanlarına göre sılanmış.Puanları eşit takımlar averajlarına göre averajları eşit takımların gol atma sayılarına göre sıralaması olmustur.
void func3(int dizigenel[BOYUT][10]) {
	int i, j, t;

	for (i = 0; i < BOYUT; i++) {
		for (j = i + 1; j < BOYUT; j++) {
			if (dizigenel[j][9] >= dizigenel[i][9]) {
				for (int k = 0; k < 10; k++) {
					t = dizigenel[i][k];
					dizigenel[i][k] = dizigenel[j][k];
					dizigenel[j][k] = t;
				}
			}

			if (dizigenel[i][9] == dizigenel[j][9]) {
				if (dizigenel[j][8] >= dizigenel[i][8]) {
					for (int k = 0; k < 10; k++) {
						t = dizigenel[i][k];
						dizigenel[i][k] = dizigenel[j][k];
						dizigenel[j][k] = t;
					}
				}
			}

			if (dizigenel[i][9] == dizigenel[j][9]) {
				if (dizigenel[j][8] == dizigenel[i][8]) {
					if (dizigenel[j][6] >= dizigenel[i][6]) {
						for (int k = 0; k < 10; k++) {
							t = dizigenel[i][k];
							dizigenel[i][k] = dizigenel[j][k];
							dizigenel[j][k] = t;
						}
					}
				}
			}
		}
	}
}

int main() {
	int dizi1[BOYUT];
	const char* dizi[BOYUT] = { "Manchester City","Bayern Munchen","Real Madrid","Paris Saint Germain","Barcelona","Liverpool","Chelsea","Manchester United","Napoli","Arsenal"
						,"Inter","Roma","Sevilla","Juventus","Dordmund","Atletico Madrid","Benfica","Ajax","Leipzig","Porto",
						 "Fenerbahce","Galatasaray","Besiktas","Villeral","Valecia","Milan","Atalanta","Frankurt","Tothenham","Bayer Leverkusen" };


	int bosluk[BOYUT];//takım isimlerinin karakter uzunluğu
	for (int i = 0; i < BOYUT; i++)
	{
		bosluk[i] = strlen(dizi[i]);
	}

	int g[BOYUT] = { 0 };//galibiyet sayısı
	int b[BOYUT] = { 0 };//beraberlik sayısı
	int y[BOYUT] = { 0 };//yenilgi sayısı
	int at[BOYUT] = { 0 };//atılan gol
	int yen[BOYUT] = { 0 };//yenilen gol
	int say;//oynanan maç sayısı

	srand(time(NULL));
	for (int i = 0; i < BOYUT; i++)//i iç saha takımlarınıj dış saha takımlarını temsil ederek birbirleriyle eşleşme sağlanır.i=j olduğu zaman yapılmaması için if deyimi yazılır.
	{
		say = 0;

		for (int j = 0; j < BOYUT; j++)
		{

			dizi1[i] = func2(i);
			dizi1[j] = func2(j);

			//İç Saha Maçları
			if (i != j)//aynı takımların eşleşmesini önlüyoruz
			{
				if (dizi1[i] > dizi1[j])
				{
					g[i]++;
					say++;
				}

				else if (dizi1[i] == dizi1[j])
				{

					b[i]++;
					say++;
				}

				else if (dizi1[i] < dizi1[j])
				{
					y[i]++;
					say++;
				}

				//Dış Saha Maçları
				if (dizi1[j] > dizi1[i])
				{
					g[j]++;
					say++;
				}

				else if (dizi1[j] == dizi1[i])
				{
					b[j]++;
					say++;
				}

				else if (dizi1[j] < dizi1[i])
				{
					y[j]++;
					say++;
				}


				if (i != j)
				{
					at[i] += dizi1[i];//iç sahada atılan gol
					yen[i] += dizi1[j];//iç sahada yenilen gol
					at[j] += dizi1[j];//dış sahada atılan gol
					yen[j] += dizi1[i];//dış sahada yenilen gol
				}
			}
		}
	}

	int av[BOYUT];
	int puan[BOYUT];

	for (int i = 0; i < BOYUT; i++)
	{
		puan[i] = 3 * g[i] + b[i];//puan
		av[i] = at[i] - yen[i];//averaj
	}

	int dizigenel[BOYUT][10];//bütün verilerin tutulduğu tablo

	for (int i = 0; i < BOYUT; i++)
	{
		dizigenel[i][0] = (int)dizi[i];//string karakterli takımların diziye dahil edebilmek için tür dönüştürücü
		dizigenel[i][1] = bosluk[i];//takım isimlerinin karakter sayısı
		dizigenel[i][2] = say;//maç sayısı
		dizigenel[i][3] = g[i];//galibiyet sayısı
		dizigenel[i][4] = b[i];//beraberlik sayısı
		dizigenel[i][5] = y[i];//yenilgi sayısı
		dizigenel[i][6] = at[i];//atılan gol sayısı
		dizigenel[i][7] = yen[i];//yenilen gol sayısı
		dizigenel[i][8] = av[i];//averaj sayısı
		dizigenel[i][9] = puan[i];//toplanan puan

	}

	func3(dizigenel);//fonksiyon 3e göre veriler sıralanır

	int gU[BOYUT]; //galibiyet sayısı karakter 
	int bU[BOYUT];//berebaerlik sayısı karakter
	int yU[BOYUT];//yenilgi sayısı karakter
	int atU[BOYUT];//atılan gol karakter sayısı
	int yenU[BOYUT];//yenilen gol karakter sayısı
	int avU[BOYUT];//averaj karakter sayısı
	int sayi[BOYUT];//oynanan maç karakter sayısı

	for (int i = 0; i < BOYUT; i++)//yazdırma işlemi için gerekli bosluklar doğru yazılsın diye boşluk ayarı için hesaplamalar yapılır
	{
		sayi[i] = snprintf(NULL, 0, "%d", i + 1);
		gU[i] = snprintf(NULL, 0, "%d", dizigenel[i][3]);
		bU[i] = snprintf(NULL, 0, "%d", dizigenel[i][4]);
		yU[i] = snprintf(NULL, 0, "%d", dizigenel[i][5]);
		atU[i] = snprintf(NULL, 0, "%d", dizigenel[i][6]);
		yenU[i] = snprintf(NULL, 0, "%d", dizigenel[i][7]);
		avU[i] = snprintf(NULL, 0, "%d", abs(dizigenel[i][8]));

	}

	printf("\n");
	printf("\033[36mPUAN DURUMU              O     G     B    M    A    Y    Ave   P \033[0m\n");

	for (int i = 0; i < BOYUT; i++)
	{
		if (i < BOYUT - 3)
		{
			printf("%d)%*s %s %*s %d %*s %d %*s  %d %*s %d %*s %d %*s %d %*s %*s%*s%d %*s %d\n", i + 1, 2 - sayi[i], "", dizigenel[i][0], 19 - dizigenel[i][1], "", dizigenel[i][2], 2, "", dizigenel[i][3], 3 - gU[i], "", dizigenel[i][4], 3 - bU[i], "", dizigenel[i][5], 3 - yU[i], "", dizigenel[i][6], 3 - atU[i], "", dizigenel[i][7], 3 - yenU[i], "", 0, dizigenel[i][8] == 0 ? " " : "", 0, dizigenel[i][8] > 0 ? "+" : "", dizigenel[i][8], 3 - avU[i], "", dizigenel[i][9]);
		}

		if (i == BOYUT - 3 || i == BOYUT - 2 || i == BOYUT - 1)
		{
			printf("\033[31m%d)%*s %s %*s %d %*s %d %*s  %d %*s %d %*s %d %*s %d %*s %*s%*s%d %*s %d\033[0m\n", i + 1, 2 - sayi[i], "", dizigenel[i][0], 19 - dizigenel[i][1], "", dizigenel[i][2], 2, "", dizigenel[i][3], 3 - gU[i], "", dizigenel[i][4], 3 - bU[i], "", dizigenel[i][5], 3 - yU[i], "", dizigenel[i][6], 3 - atU[i], "", dizigenel[i][7], 3 - yenU[i], "", 0, dizigenel[i][8] == 0 ? " " : "", 0, dizigenel[i][8] > 0 ? "+" : "", dizigenel[i][8], 3 - avU[i], "", dizigenel[i][9]);
		}
	}
	printf("\n");

	return 0;

}

