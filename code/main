import pandas as pd
import pandas_datareader as dr
from datetime import datetime
import matplotlib.pyplot as plt

def average(values,coef):
    averg=0
    k1=coef
    values=values[:coef]
    average_value=sum(values)/k1
    return (average_value)  

def correlation(aver_x,aver_y,data_x,data_y,period):
    oper1=0
    alfa1=[]
    alfa2=[]
    alfa3=[]
    for i in range(period):
      alfa1.append((data_x[i]-aver_x)*(data_y[i]-aver_y))
      alfa2.append((data_x[i]-aver_x)**2)
      alfa3.append((data_y[i]-aver_y)**2)
    correlation=sum(alfa1)/((sum(alfa2)*sum(alfa3))**0.5)   
    return (correlation)

def standard_deviation(values,coef,average):
    oper1=0
    for i in range(coef):
      oper1+=(values[i]-average)**2
    stand_derivation=(oper1/coef)**0.5
    return (stand_derivation)

def graph_plot(ratio, boll_inf, boll_sup, average, x, name):
    plot_ratio = ratio
    plot_boll_inf = boll_inf
    plot_boll_sup = boll_sup
    plot_average = average

    plot_ratio = plot_ratio[::-1]
    plot_boll_inf = plot_boll_inf[::-1]
    plot_boll_sup = plot_boll_sup[::-1]
    plot_average = plot_average[::-1]

    plt.plot(x, plot_ratio)
    plt.plot(x, plot_boll_inf)
    plt.plot(x, plot_boll_sup)
    plt.plot(x, plot_average)

    plt.title(name)  # adicionando o título
    plt.xlabel('Períodos')
    plt.ylabel('Valores')

    fig = plt.figure(1)
    rect = fig.patch
    rect.set_facecolor("white")
    plt.savefig(name + ".png", facecolor=fig.get_facecolor())
    plt.show()

aux_sa='.sa'
correlation_stock1=[]
correlation_stock2=[]
ratio=[]
average40=[]
standard_deviation40=[]
boll_sup=[]
boll_inf=[]
values_graph=[]

date=datetime.today().strftime('%Y-%m-%d')

for i in range(30):
    values_graph.append(i)  # fazer uma lista com datas

print(" -- INSTRUÇÕES PARA USO DO ALGORÍTIMO -- \n")
print("1 - Insira o nome do ativo da seguinte maneira: petr4, itub4 e elet3.")
print("2 - Insira o nome do ativo e pressione ENTER.")
print("3 - Os valores apresentam atraso em relação ao mercado.\n")

stocks1_1=input("Ativo Long (Exemplo: petr4): ")
stocks2_1=input("Ativo Short (Exemplo: petr3): ")
print('')

stocks1=stocks1_1+aux_sa
stocks2=stocks2_1+aux_sa

df1=dr.data.get_data_yahoo(stocks1,  end=date, start='2019-01-02')
df1 = df1.iloc[::-1]

df2=dr.data.get_data_yahoo(stocks2,  end=date, start='2019-01-02')
df2 = df2.iloc[::-1]

size_len1=len(df1)
size_len2=len(df2)

if size_len1>=size_len2:
    range_k=size_len2-1
else:
    range_k=size_len1-1

#correlação
for k1 in range(range_k):
  correlation_stock1.append(((df1.iloc[k1,3]/df1.iloc[k1+1,3])-1)*100)
  correlation_stock2.append(((df2.iloc[k1,3]/df2.iloc[k1+1,3])-1)*100)

average_x45=average(correlation_stock1,45)
average_y45=average(correlation_stock2,45)
average_x90=average(correlation_stock1,90)
average_y90=average(correlation_stock2,90)

correlation45=correlation(average_x45,average_y45,correlation_stock1,correlation_stock2,45)  #correlação 45 dias
correlation90=correlation(average_x90,average_y90,correlation_stock1,correlation_stock2,90)  #correelação 90 dias

print(f"Correlação 45 dias {correlation45:.3f}.\nCorrelação 90 dias {correlation90:.3f}.\n")

for k2 in range(range_k):
    ratio.append(df1.iloc[k2,3]/df2.iloc[k2,3])

for k3 in range(range_k):   
    average40.append(average(ratio[k3:40+k3],40))

for k4 in range(range_k-49): 
   standard_deviation40.append(standard_deviation(ratio[k4:40+k4],40,average40[k4]))

for k5 in range(range_k-49):
    boll_sup.append(average40[k5]+(2*standard_deviation40[k5]))
    boll_inf.append(average40[k5]-(2*standard_deviation40[k5]))  
   
if ratio[0]<=boll_inf[0]:
    print(f"Comprar {stocks1} e vender {stocks2}.\n")       
elif ratio[0]/tolerance>=boll_sup[0]:
    print(f"Comprar {stocks2} e vender {stocks1}.\n")         

graph_plot(ratio[:30],boll_inf[:30],boll_sup[:30],average40[:30],values_graph,stocks1+'-'+stocks2+"  - "+date_saopaulo2+" "+hour_saopaulo) 

print("")
print(f"LEGENDA: Valores referente ao ratio {stocks1_1}/{stocks2_1} .\n")
print(f"Linha azul: ratio atual.")
print(f"Linha vermelha: média móvel 40 períodos.")
print(f"Linha verde: banda de banda de bollinger superior.")
print(f"Linha amarela: banda de banda de bollinger inferior.")
