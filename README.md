# Socioeconomic-Indicators-in-Chicago

%load_ext sql

dsn_driver = "{IBM DB2 ODBC DRIVER}"
dsn_database = "BLUDB"              
dsn_hostname = "dashdb-txn-sbox-yp-dal09-04.services.dal.bluemix.net"            
dsn_port = "50000"                  
dsn_protocol = "TCPIP"            
dsn_uid = "lxw19392"               
dsn_pwd = "21bsc1hgx7t7g+1l"  
%sql ibm_db_sa://lxw19392:21bsc1hgx7t7g%2B1l@dashdb-txn-sbox-yp-dal09-04.services.dal.bluemix.net:50000/BLUDB

import pandas
chicago_socioeconomic_data = pandas.read_csv('https://data.cityofchicago.org/resource/jcxq-k9xf.csv')
%sql PERSIST chicago_socioeconomic_data

%sql SELECT * FROM chicago_socioeconomic_data limit 5;

# How many rows are in the dataset

%sql SELECT COUNT(*) FROM chicago_socioeconomic_data

# How many community areas in Chicago have a hardship index greater than 50.0?

%sql SELECT COUNT(*) FROM chicago_socioeconomic_data WHERE hardship_index > '50'

# What is the maximum value of hardship index in this dataset?

%sql SELECT MAX (hardship_index) FROM chicago_socioeconomic_data

# Which community area which has the highest hardship index?

%sql SELECT community_area_name FROM chicago_socioeconomic_data where hardship_index = (SELECT MAX (hardship_index) FROM chicago_socioeconomic_data)

# Which Chicago community areas have per-capita incomes greater than $60,000?

%sql SELECT community_area_name FROM chicago_socioeconomic_data where per_capita_income_ > 60000

# Create a scatter plot using the variables per_capita_income_ and hardship_index. Explain the correlation between the two variables.

import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns

income_vs_hardship = %sql SELECT per_capita_income_, hardship_index FROM chicago_socioeconomic_data;
plot = sns.jointplot(x='per_capita_income_',y='hardship_index', data=income_vs_hardship.DataFrame())

