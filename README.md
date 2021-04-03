# Sharks Attacks Worldwide 2010 - 2020 (White Shark, Tiger Shark, Bull Shark)
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings('ignore')
%matplotlib inline
pd.options.display.float_format = '{:.2f}'.format

df = pd.read_excel('GSAF5.xls')
df.head()

df.info()
df['Fatal (Y/N)'].value_counts(dropna=False)
df['Fatal (Y/N)'] = df['Fatal (Y/N)'].str.replace('\d+', '').str.replace(' ','').str.replace('x','').str.upper()
df['Fatal (Y/N)'].str.contains(['Y','N'],regex=False)
df['Species '].value_counts()

chars = ["!",'"',"#","%","&","'","(",")",
              "*","+",",","-",".","/",":",";","<",
              "=",">","?","@","[","\\","]","^","_",
              "`","{","|","}","~","–"]

for char in chars:
    df['Species '] = df['Species '].str.replace(char, '')
    
df['Species '].value_counts()
df['Species '] = df['Species '].str.replace('\d+', '').str.replace('m','').str.replace('Juvenile','')
df['Species '] = df['Species '].str.replace('x','').str.replace('female','')
df['Species '] = df['Species '].str.replace('Small','').str.replace('Possibly a','').str.replace('kg','')
df['Species '] = df['Species '].str.replace('feet','').str.replace('to','').str.replace('lb','')
df['Species '] = df['Species '].str.replace('Said to involve a','').str.title()
df['Species '].value_counts()

cols_to_include = ['Year','Type','Country', 
                   'Area','Location',
                   'Activity', 'Fatal (Y/N)',
                   'Species ']

renaming = {'Area':'State', 
            'Species ':'Species',
           'Fatal (Y/N)':'Fatal'}

df1 = df[cols_to_include].rename(renaming, axis=1)
key_vars = ['Type','Country','State','Location','Activity','Fatal','Species']
sharks = df1[df1.Year.isin(range(2010,2021))]
sharks.head()

pd.set_option('max_columns',None)
pd.set_option('max_rows',None)
sharks.sort_values(by=['Year'],ascending=False).reset_index(drop=True)
sharks

sharks['Species'].describe()
sharks['Species'].value_counts()
sharks['Species'] =sharks['Species'].str.replace('\d+', '').str.replace('M','').str.split().str.join(' ')
sharks['Species'].value_counts(normalize=True)
sharks['Species'] =sharks['Species'].str.replace('s','').str.replace('Sall','').str.replace('Pup','').str.replace(
    'Juvenile','')

# I did some research online Google(e.g. news articles) to fill in all the NaN values 
white_shark = sharks[sharks['Species']=='White Shark']
pd.set_option('max_columns',None)
pd.set_option('max_rows',None)
white_shark

white_shark.iloc[[8,17,56],[6]] = 'N'
white_shark.iloc[[10],[4]] = 'Stanley'
white_shark.iloc[[56],[3]] = 'Baja California'
white_shark.iloc[[59],[5]] = 'Free diving'
white_shark.iloc[[77],[4]] = 'Jeffreys Bay'
white_shark.iloc[[85],[4]] = 'Port St. Johns'
white_shark.iloc[[89],[3,4]] = 'Atlantic Ocean'
white_shark.iloc[[93],[4]] = 'Fremantle'
white_shark.iloc[[94],[5]] = 'Boat attack'
white_shark.iloc[[102],[3,4]] = 'South Africa'
white_shark.iloc[[104],[3]] = 'Southland'
white_shark.iloc[[107],[4]] = 'Albatros Point, near Jeffreys Bay'
white_shark.iloc[[147],[4]] = 'Turners Beach'
white_shark

white_shark.isnull().sum()
white_shark.info()
white_shark.shape
white_shark['Fatal'].count()
white_shark[white_shark['Fatal']=='Y'].count()

cols_to_include_1 = ['Year','Type','Country','State','Location','Activity', 'Fatal','Species']
df1= white_shark[cols_to_include_1]
key_vars = ['Type','Country','State','Location','Activity','Fatal','Species']
white_shark_df = df1[df1.Year.isin(range(2010,2021))]
white_shark_df.head()

sns.countplot(x='Species', hue='Fatal',data=white_shark_df)

tiger_shark = sharks[sharks['Species']=='Tiger Shark']
tiger_shark

tiger_shark.iloc[[0],[3]] = 'Caribbean'
tiger_shark.iloc[[7],[3,4,6]] = ['Saint-Paul','Saline-les-Bains','Y']
tiger_shark.iloc[[11],[3]] = 'Oceania'
tiger_shark.iloc[[18],[3]] = 'Red Sea'
tiger_shark.iloc[[25],[4]] = 'Hultins Beach, Oahu'
tiger_shark.iloc[[43],[3,4]] = ['Oceania','Fiji']
tiger_shark.iloc[[50],[4]] = 'Riecks Point, Campbells Beach'
tiger_shark.iloc[[52],[4]] = "Pila'a Beach, Kauai"
tiger_shark.iloc[[53],[4]] = 'Makena, Maui'
tiger_shark.iloc[[59],[4]] = 'Port St. Johns'
tiger_shark.iloc[[63],[4]] = 'Davidsons Surf Break, Kekaha, Kauai'
tiger_shark.iloc[[67],[4]] = 'Leftovers near Chuns Reef, Oahu'
tiger_shark.iloc[[69],[3]] = 'Bahamas'
tiger_shark

tiger_shark.info()
tiger_shark.isnull().sum()
tiger_shark.shape
tiger_shark['Fatal'].count()
tiger_shark[tiger_shark['Fatal']=='Y'].count()

cols_to_include_2 = ['Year','Type','Country','State','Location','Activity', 'Fatal','Species']
df2= tiger_shark[cols_to_include_2]
key_vars = ['Type','Country','State','Location','Activity','Fatal','Species']
tiger_shark_df = df2[df2.Year.isin(range(2010,2021))]
tiger_shark_df.head()

sns.countplot(x='Species', hue='Fatal',data=tiger_shark_df)

bull_shark = sharks[sharks['Species']=='Bull Shark']
bull_shark

bull_shark.iloc[[12],[5]] = 'Fish farm'
bull_shark.iloc[[15],[3]] = 'Bahamas'
bull_shark.iloc[[17],[5]] = 'Spearfishing'
bull_shark.iloc[[18],[3,4]] = ['Oceania','Nouvelle']
bull_shark.iloc[[21,27,33],[3]] = 'Saint-Paul'
bull_shark.iloc[[33],[3]] = 'Bahamas'
bull_shark.iloc[[35],[3,4]] = ['Saint-Paul','Saint-Leu']
bull_shark.iloc[[41],[3]] = 'Oceania'
bull_shark.iloc[[44],[4]] = 'Nerang River, Surfers Paradise'
bull_shark.iloc[[49],[3,4]] = ['Bahamas','Grand Bahamas']
bull_shark.iloc[[59],[4]] = 'Kylies Beach, Diamond Head'
bull_shark.iloc[[61],[3,4]] = ['Saint-Paul','Saint-Leu']
bull_shark.iloc[[63],[4]] = 'Nobbys Beach'
bull_shark

bull_shark.isnull().sum()
bull_shark['Fatal'].count()
bull_shark[bull_shark['Fatal']=='Y'].count()

cols_to_include_3 = ['Year','Type','Country','State','Location','Activity', 'Fatal','Species']
df3= bull_shark[cols_to_include_3]
key_vars = ['Type','Country','State','Location','Activity','Fatal','Species']
bull_shark_df = df3[df3.Year.isin(range(2010,2021))]
bull_shark_df.head()

sns.countplot(x='Species', hue='Fatal',data=bull_shark_df)

frames = [white_shark_df,tiger_shark_df,bull_shark_df]
shark_df = pd.concat(frames,sort=True)
shark_df

shark_df.info()
shark_df.columns
shark_df.shape
shark_df['Fatal'].count()

sns.countplot(x='Species', hue='State',data=shark_df)
plt.legend(bbox_to_anchor=(1.01, 1),borderaxespad=0)

sns.countplot(x='Species', hue='Country',data=shark_df)
plt.legend(bbox_to_anchor=(1.01, 1),borderaxespad=0)

sns.countplot(x='Species',hue='Fatal',data=shark_df)

shark_df[shark_df['Fatal']=='Y'].count()
shark_df.isnull().sum()
shark_df.apply(lambda x: len(x.unique()))

#pd.set_option('max_columns',None)
#pd.set_option('max_rows',None)
#shark_df
shark_df.sort_values(by=['Year'],ascending=False).reset_index(drop=True)
shark_df['State'].unique()
shark_df['Location'].unique()

# All the coordinates for these Location's I retrived from Google one by one to use futher down in the code Geopandas
coordinates = ({'Location': [ 
        'Coronado, San Diego County','Pauanui Beach','Kangaroo Island',
        'Stanmore Bay, Auckland','Sand Dollar Beach, Santa Cruz County','Salt Beach near Kingscliff', 
        'Cabarita Beach','Greenmount Beach, Coolangatta','Forster',
        'Shelly Beach, Port Macquarie','Bunker Bay Beach','Tenth Island',
        'Bailey Island, Cumberland County','Wilsons Headland', 'Stanley',
        'Shelter Cove, Humboldt County','Cull Island / Esperance','Orient Beach',
        'Neuse River, Craven County','Cocoa Beach, Brevard  County','Chebilang pier ',
        'Lucinda','Honolua Bay','Homestead, Miami-Dade County',
        'Melbourne Beach, Brevard County','NoName Cay','Sombero Key Light, Monroe County',
        'Wailea','Davidsons Beach, Kauai','Plateau de Ricaudy, Nouméa',
        'Cable Beach','St. Vincent Bay','Key Biscayne',
        'Sandspit Beach, Montaña de Oro State Park ','Lighthouse Beach','Ship Rock near Catalina, Los Angeles County',
        'Burns Beach, Perth', 'North Salmon Creek Beach, Sonoma County','San Onofre, San Diego County',
        'Pitt Island or Chatham Island','Cove Park, Kihei, Maui','Nerang River, Surfers Paradise',
        'Saline-les-Bains','Noumea','North Kohala, Big Island',
        'Anaehoomalu Bay, Hawaii','Beqa','Off Airlie Beach, Whitsundays',
        'Shellharbour','East London','L’Hermitage Lagoon',
        'Wilmington River, Chatham County','Samurai Beach','Madoogali',
        'Marsa Alam','Surf Beach, Kiama','Martin Islet',
        'Little Congwong Beach, La Perouse ','Cocoa Beach, Brevard County','Cone Bay',
        'Nouvelle','Bimini','Sai Noi Beach',
        'La Ticla','Newcomb Hollow Beach, Wellfleet, Barnstable County','South Point, Gracetown',
        'Groote Eylandt','Longnook Beach, Truro, Barnstable County','Nirvana Beach',
        'Beacons Beach, San Diego County','Moffat Beach','Piedade Beach, Recife',
        'Baylys Beach ','Manuelita','Pounders Beach, Oahu', 
        'Oceanside, San Diego County','Kukio Beach','Southeast of Puerto Peñasco ',
        'St. Francis Bay','Robberg Beach, Plettenberg Bay','Tinley Manor',
        'Humboldt County','Point Casuarina, Bunbury','Athol Island',
        'Sánchez Magallanes, Cárdenas','Davidsons Beach, Kekaha, Kauai','Guardalavaca Beach', 
        'Cathedral Rock','Between Pescadero Point & Bean Hollow Beach, San Mateo County','South Beach, Westport, Grays Harbor County',
        'Nahoon Reef, East London','Seal Rock, Goleta Beach, Santa Barbara','Monterey Bay',
        'Kelpies near Wylie Bay','Santa Cruz, Santa Cruz County','Kekaha Beach, Kauai',
        'Marconi Beach, Wellfleet, Barnstable County','Eastmoor Crescent Beach','Hultins Beach, Oahu',
        'Haulover Beach, Miami-Dade County','Stillwater Cove, Monterey County','Normanville', 
        'Esperance','Iluka Beach','Off Jupiter', 
        'Stearns Wharf, Santa Barbara','Gracetown','Balian Beach',
        'Mauds Point','Destin, Okaloosa County','Roches Noires',
        'Burkes Beach, Hilton Head, Beaufort County','Boot Reef, Torres Strait','Robberg Beach, Plettenberg Bay',
        'Maui','Capitola, Santa Cruz County','Falcon Beach, Mandurah',
        'Off Palos Verdes peninsula, Los Angeles County','Refugio State Beach, Santa Barbara County','Surfside, Orange County',
        'Near Albany','Lighthouse Beach, Ballina','Ryspunt', 
        'Makaha, Oahu','Off Singer Island, Palm Beach County','Balian',
        'Kamaole Beach Park I, Maui','8 miles off Mobile','Poe Beach',
        'Green Turtle Cay','Boucan Canot','New Smyrna Beach, Volusia County',
        'St. Petersburg, Pinellas County','Off Surfside','Keurbooms Lagoon, Plettenberg Bay', 
        'Booti Booti National Park','Guadalupe Island','Wailea Beach, Maui',
        'Stil Bay','Fernano de Noronha','Saint-Leu',
        'Nerang River, Surfers Paradise','East Ballina','Off Andros Island',
        'Oak Island, Brunswick County','Lori Wilson Park, Cocoa Beach, Brevard  County','Folette',
        'Kouare ','Hapuna Beach','Cap Homard',
        '3 miles off Jupiter, Palm Beach County','Westbrook Beach','Morro Strand State Beach, San Luis Obispo County', 
        'Ravine Mula','Fiji','Rodanthe, Dare County',
        'Leftovers, Oahu','Lanikai Beach, Kailua, Oahu','Hallidays Point',
        'Upolu Point, North Kohala, Big Island','Lake Macquarie','El Pescador Beach, Los Angeles County',
        'Denmark','Horseshoe Rock, Santa Barbara County','Saltwater Beach',
        'Lookout Beach, Plettenberg Bay','Buffels Bay near Knysna','Port St.Johns',
        'Belongil Beach, Byron Bay','Yellow Sands Point','Atlantic Ocean',
        'Off Panama City','Fishery Bay','Oak Island, Brunswick County',
        'Lennox Head','Huntington Beach, Orange County','Jeffreys Bay',
        'Lachan Island, Mercury Passage','Evans Head','Santa Barbara County',
        'Morro Bay, San Luis Obispo County','East Ballina','Muizenberg',
        'Katrina Cut, Dauphin Island, Mobile County','South Africa','Elliston, Eyre Peninsula',
        'Manhattan Beach, Los Angeles County','Fremantle','Manomet Point, Plymouth, Plymouth County',
        'Kelpids Beach, Wylie Bay, Esperance','Santa Barbara County','Santa Barbara County',
        'Hilton Head Island, Beaufort County','Stewart Island','Lake Ponchartain off Southshore Harbor, New Orleans',
        'Montaña de Oro State Park, San Luis Obispo County ','Tathra','Kihei, Maui',
        'Apalachicola Bay ','North Kohala, Hawaii County','Rudder Reef',
        'Franklin Point, San Mateo County','Three Stripes near Cheynes Beach','Nerang River near Chevron Island',
        'Fort Lauderdale','Grand Bahamas','3 to 4 miles west of Indian Pass, Gulf County',
        'Die Platt',"Pila'a Beach, Kauai",'Kanaha Beach, Maui',
        'Riecks Point, Campbells Beach','Ninole Bay, Hawaii County','Seagull Beach, Cancun',
        'Muriwai','Pacific State Beach, San Mateo County','Pillar Point, Half-Moon Bay, San Mateo County',
        'Off Poison Creek, Cape Arid','Albatros Point, near Jeffreys Bay','Cape Nelson',
        'Bunkers, Humboldt Bay, Eureka, Humboldt County','Kiholo Bay','Brisant Beach',
        'White Plains Beach, Oahu','Casino Beach, Pensacola, Escambia County','Folly Beach',
        'Jacksonville, Duval County','Sanibel Island, Lee County','New Smyrna Beach, Volusia County',
        'Gleneden Beach, Lincoln County','Makena, Maui','Pillikin Red Light area ',
        'Kona Coast State Park','Makena Landing, Maui',' Boca de la Leña, La Unión',
        'Kihei, Maui','Coral Bay','Cat Island', 
        'Spreckelsville, Maui','Off Catalina Island','Davidsons Surf Break, Kekaha, Kauai', 
        'Nobbys Beach','Makena Landing, Maui','Trigg Beach', 
        'Eueiki Island','Jensen Beach, Martin County ','Caves near Kogel Bay', 
        'Port St.Johns','Dolphin Bay, Innes National Park','Off Wedge Island',
        'Stratham Beach','Kylies Beach, Diamond Head','Saint-Leu', 
        'Leftovers near Chuns Reef, Oahu','Humboldt Bay, Eureka, Humboldt County','Surf Beach, Lompoc, Santa Barbara County',
        'Iroquiois Point, Oahu','Redhead Beach, Newcastle','Lincoln City, Lincoln County',
        'Redhead Beach', 'Kalbarri','Strandfontein',
        'Leffingwell Landing, Cambria,  San Luis Obispo County','Cintza Beach, East London','Pigeon Point',
        'Hula, near Port Moresby','Busselton','Off Perforated Island near Coffin Bay',
        'Clovely Beach','Lookout Beach, near the Keurbooms river mouth in Plettenberg Bay','Newport, Lincoln County',
        'Rottnest Island','Marina State Beach, Monterey County',"Pila'a Beach, Kauai",
        'Lyman Beach, Kailua-Kona','Robberg Beach','Kendec',
        ' Bunker Bay','North Topsail Beach, Onslow County','Santa Maria Island, Manatee County',
        'Between ','Crowdy Head','Gaviotas Beach, Cancun',
        'Balian','Anse Lazio ','Riviera Beach, Palm Beach County',
        'Angourie','Cowaramup Bay','Winchester Bay', 
        'Between Dyer Island and Pearly Beach','Turners Beach','Pigeon Point, San Mateo County',
        'Near oil rig Hondo, 5 nm from Gaviota, Santa Barbara County','Surf Beach, Vandenberg AFB, Santa Barbara County','Fish Hoek',
        'Mullaway Headland','Off Garden Island','Between Carnac and Garden Islands',
        'Eight Mile Beach, Galveston'],
     
     'State': [
         'California','North Island','South Austrialia',
         'North Island','California','New South Wales',
         'New South Wales','Queensland','New South Wales',
         'New South Wales','Westerm Australia','Tasmania',
         'Maine','New South Wales','Tasmania',
         'California','Westerm Australia','Caribbean',
         'North Carolina','Florida','Muang district of Satun province',
         'Queensland','Hawaii','Florida',
         'Florida','Abaco Islands','Florida',
         'Maui','Hawaii','South Province',
         'Western Australia','South Province','Florida',
         'California','New South Wales','California',
         'Westerm Australia','California','California',
         'Chatham Islands','Hawaii','Hawaii',
         ' Saint-Paul','Oceania','Hawaii',
         'Hawaii','Oceania','Queensland',
         'New South Wales','Eastern Cape Province','Saint-Gilles',
         'Georgia','New South Wales','Alifu Alifu Atoll',
         'Red Sea','New South Wales','New South Wales',
         'New South Wales','Florida','Western Australia',
         'Oceania','Bahamas','Hua Hin',
         'Colima','Massachusetts','Western Australia',
         'Northern Territory','Massachusetts','New Providence',
         'California','Queensland','Pernambuco',
         'North Island','Cocos Island','Hawaii',
         'California','Hawaii','Sonora',
         'Eastern Cape Province','Western Cape Province','KwaZulu-Natal',
         'California','Western Australia','New Providence ',
         'Tabasco','Hawaii','Holquin Province',
         'Victoria','California','Washington',
         'Eastern Cape Province','California','California',
         'Western Australia','California','Hawaii',
         'Massachusetts','KwaZulu-Natal','Hawaii',
         'Florida','California','South Australia',
         'Western Australia','New South Wales','Florida',
         'California','Western Australia','Bali',
         'Western Australia','Florida','Saint-Paul',
         'South Carolina','Queensland','Western Cape Province',
         'Hawaii','California','Western Australia',
         'California','California','California', 
         'Western Australia','New South Wales','Western Cape Province',
         'Hawaii','Florida','Bali',
         'Hawaii', 'Alabama','Grand Terre',
         'Abaco Islands','Saint-Paul','Florida',
         'Florida', 'Texas','Western Cape Province',
         'New South Wales','Baja California','Hawaii',
         'Western Cape Province','Pernambuco','Saint-Paul',
         'Queensland','New South Wales','Bahamas',
         'North Carolina','Florida','Le Port',
         'Oceania', 'Hawaii','Saint-Gilles-les-Bains',
         'Florida','KwaZulu-Natal','California',
         'd’Étang-Salé','Oceania','North Carolina',
         'Hawaii','Hawaii','New South Wales',
         'Hawaii','New South Wales','California',
         'Western Australia','California','New South Wales',
         'Western Cape Province','Western Cape Province','Eastern Cape Province',
         'New South Wales','Eastern Cape Province','Atlantic Ocean',
         'Florida','South Australia','North Carolina',
         'New South Wales','California','Eastern Cape Province',
         'Tasmania','New South Wales','California',
         'California','New South Wales','Western Cape Province',
         'Alabama','South Africa','South Australia',
         'California','Western Australia','Massachusetts',
         'Western Australia','California','California',
         'South Carolina','Southland','Louisiana',
         'California','New South Wales','Hawaii',
         'Florida','Hawaii','Queensland',
         'California','Western Australia','Queensland',
         'Florida','Bahamas','Florida',
         'Western Cape Province','Hawaii','Hawaii',
         'New South Wales','Hawaii','Quintana Roo',
         'North Island','California','California',
         'Western Australia','Eastern Cape Province','Victoria',
         'California','Hawaii','Saint-Gilles',
         'Hawaii','Florida','South Carolina',
         'Florida','Florida','Florida',
         'Oregon','Hawaii','St. Catherine',
         'Hawaii','Hawaii','Guerrero',
         'Hawaii', 'Western Australia','Bahamas',
         'Hawaii','California','Hawaii',
         'Queensland','Hawaii','Western Australia',
         "Vava'u",'Florida','Western Cape Province',
         'Eastern Cape Province','South Australia','Western Australia',
         'Western Australia','New South Wales','Saint-Paul',
         'Hawaii','California','California',
         'Hawaii','New South Wales','Oregon',
         'New South Wales','Western Australia','Western Cape Province',
         'California','Eastern Cape Province','California',
         'Central Province','Western Australia','South Australia',
         'Western Cape Province','Western Cape Province','Oregon',
         'Western Australia','California','Hawaii',
         'Hawaii','Western Cape Province','North Province',
         'Western Australia','North Carolina','Florida',
         'Queensland','New South Wales','Quintana Roo',
         'Bali','Praslin','Florida',
         'New South Wales','Western Australia','Western Australia',
         'Western Australia','Oregon','California',
         'California','California','Western Cape Province',
         'New South Wales','Western Australia','Western Australia',
         'Texas'],
     
     'Latitude': [32.6859,-37.0161,-35.7752,
                 -36.6264,35.9232,-28.2777,
                 -28.3436,-28.1640,-32.1806,
                 -33.3700,-33.5441,-40.9423,
                 43.7367,-29.8593,-40.7597,
                 40.0304,-33.9220,18.0875,
                 35.1313,28.3200,6.7014,
                 -18.5313,21.0139,25.4615,
                 28.0789,26.7490,24.6287,
                 20.6828,21.9642,-22.3138,
                 -17.9320,-21.9650,25.6937,
                 35.3033,-31.4789,33.4625,
                 -31.7247,38.3592,33.3728,
                 -44.2899,20.7273,-28.0167,
                 -21.0927,-22.2735,20.0917,
                 19.5458,-18.3828,-20.2678,
                 -34.5833,-33.0292,-21.0795,
                 31.9552,-32.7678,4.0960,
                 25.0676,-34.6769,-34.4943,
                 -33.9916,28.3200,-16.4683,
                 -20.9043,25.7333,12.5684,
                 18.4544,41.9632,-33.8645,
                 -13.9700,42.0209,25.0637,
                 33.0652,-26.8000,-8.1721,
                 -35.9517,5.5613,21.66327,
                 33.1959,19.8231,31.3172,
                 -34.1605,-34.0760,-29.4500,
                 40.7450,-33.3166,25.0783,
                 18.2953,21.9642,21.1221,
                 -37.4713,37.2396,46.8938,
                 -32.9958,34.8997,36.8007,
                 -33.8325,36.9741,21.9717,
                 41.8918,-29.8833,21.4389,
                 25.9114,36.5639,-35.4464,
                 -33.8613,-29.4120,26.9342,
                 34.4100,-33.8642,-8.5031,
                 -23.1206,30.3935,-21.0536,
                 32.1941,-10.0000,-34.0758,
                 20.7984,36.9752,-32.5801,
                 33.7684,34.4632,33.7271,
                 -35.0269,-28.8726,-34.3633,
                 21.4634,26.7851,-8.5031,
                 20.7222,30.6954,-21.6121,
                 26.7747,-21.0316,29.0258,
                 27.7676,28.9517,-34.0380,
                 -32.3127,29.0525,20.6828,
                 -34.3642,-3.8576,-21.1748,
                 -28.0007,-28.8537,24.7064,
                 33.9204,28.3371,-20.9444,
                 -20.9043,19.9923,-21.0368,
                 26.9342,-29.5912,35.3918,
                 -21.2498,-17.7134,35.6004,
                 21.4389,21.3925,-32.0684,
                 20.2690,-33.0311,34.0404,
                 -34.9618,34.4208,-32.0108,
                 -34.0506,-34.0833,-31.6288,
                 -28.6337,-32.9097,14.2576,
                 30.1588,-34.9142,33.9204,
                 -28.8000,33.6595,-34.0507,
                 -42.3824,-29.1086,34.4208,
                 35.3659,-28.8537,-34.0899,
                 30.2555,-30.5595,-33.6452,
                 33.8847,-32.0534,41.9161,
                 -33.8325,34.4208,34.4208,
                 32.2163,-47.0000,30.0381,
                 35.2723,-36.7360,20.7644,
                 26.6718,19.5429,-16.1942,
                 37.1497,-34.8741,-27.9973,
                 26.1224,26.6594,29.6905,
                 -33.2278,22.2102,20.8997,
                 -30.2379,19.1283,21.1619,
                 -36.8246,37.5989,37.5043,
                 -33.9036,-34.0507,-38.3596,
                 40.7195,19.8564,-21.0539,
                 21.3035,30.3304,32.6552,
                 30.3322,26.4434,29.0258,
                 44.8814,20.6316,18.1096,
                 19.7704,20.6538,17.9733,
                 20.7644,-23.1423,24.2133,
                 20.9107,33.3879,21.9642,
                 -32.9238,20.6538,-31.8779,
                 -18.7664,27.2545,-34.2342,
                 -31.6288,-35.1945,-30.8278,
                 -33.4519,-31.8144,-21.1748,
                 21.6269,40.7195,34.6831,
                 21.3275,-33.0205,44.9582,
                 -33.0205,-27.7104,-34.0779,
                 35.5839,-32.8287,37.1829,
                 -9.4438,-33.6532,-34.7268,
                 -33.9135,-34.0506,44.6368,
                 -32.0064,36.6928,22.2102,
                 19.6399,-34.0754,-20.6667,
                 -33.5427,34.4902,27.5041,
                 -20.9176,-31.8443,21.1323,
                 -8.3405,-4.2937,26.7753,
                 -29.4792,-33.8606,-32.1385,
                 -32.1755,43.6771,37.1818,
                 34.6831,34.4717,-34.1341,
                 -30.0771,-32.2043,-32.1393,
                 29.3013],
     
     'Longitude': [-117.1831,175.8677,137.2142,
                  174.7256,-121.4687,153.5804,
                  153.5732,153.5476,152.5117,
                  151.4859,115.0346,146.9849,
                  -69.9941,153.2715,145.2960,
                  -124.0731,121.9048,-63.0197,
                  -76.5038,-80.6076,99.9577,
                  146.3320,-156.6385,-80.3350,
                  -80.5606,-77.2965,-81.1092,
                  -156.4430,-159.7089,166.4557,
                  122.2096,166.1204,-80.1628,
                  -120.8750,152.9298,-118.4917,
                  115.7249,-123.0682,-117.5656,
                  -176.2260,-156.4499,153.4167,
                  55.2397,166.4481,-155.7149,
                  -155.5335,178.1194,148.7156,
                  150.8667,27.8546,55.2223,
                  -81.0044,152.1242,72.7529,
                  34.8790,150.8542,150.9378,
                  151.2384,-80.6076,123.5353,
                  165.6180,-79.2500,99.9577,
                  -103.5541,-69.9965,114.9768,
                  136.5933,-70.0370,-77.4884,
                  -117.3047,153.1333,-34.9155,
                  173.7469,-87.0477,-157.9204,
                  -117.3795,-156.0008,-113.5380,
                  24.8241,23.3722,31.2833,
                  -124.0784,115.6333,-77.2718,
                  -93.8617,-159.7089,-75.8387,
                  144.7852,-122.4178,-124.1312,
                  27.9514,-120.6732,-121.9473,
                  121.9971,-122.0308,-159.7304,
                  -69.9611,31.0499,-158.0001,
                  -80.1235,-121.9423,138.3216,
                  121.8914,153.3647,-80.0942,
                  -119.6856,114.9882,114.9656,
                  113.7615,-86.4958,55.2235,
                  -80.6977,144.6667,23.3722,
                  -156.3319,-121.9533,115.6524,
                  -118.3492,-120.0701,-118.0819,
                  117.8837,153.5899,20.1945,
                  -158.2206,-80.0375,114.9656,
                  -156.4479,-88.0399,165.4010,
                  -77.3296,55.2273,-80.9270,
                  -82.6403,-95.2875,23.3782,
                  152.5179,-118.2761,-156.4430,
                  21.4336,-32.4297,55.2879,
                  153.4296,153.5865,-78.0195,
                  -78.1608,-80.6095,55.3033,
                  165.6180,-155.8257,55.2171,
                  -80.0942,31.1713,-120.8664,
                  55.3697,178.0650,-75.4656,
                  -158.0001,-157.7151,152.5150,
                  -155.8545,151.5603,-118.8921,
                  117.3507,-119.6982,152.5629,
                  23.3776,22.9731,29.5369,
                  153.6003,28.0798,-43.449815,
                  -85.6602,135.6861,-78.1608,
                  153.5833,-117.9988,24.9102,
                  147.5812,153.4217,-119.6982,
                  -120.8500,153.5865,18.4959,
                  -88.0849,22.9375,134.8886,
                  -118.4109,115.7580,-70.5621,
                  121.9971,-119.6982,-119.6982,
                  -80.7526,167.8400,-90.0156,
                  -120.8868,149.9674,-156.4450,
                  -85.0026,-155.6659,145.6990,
                  -122.3605,118.3963,153.4198,
                  -80.1373,-78.5207,-85.2644,
                  21.8569,-159.3687,-156.4406,
                  153.1522,-155.5094,-86.8515,
                  174.4339,-122.5019,-122.4826,
                  123.3513,24.9102,141.6051,
                  -124.2426,-155.9217,55.2287,
                  -158.0452,-87.1418,-79.9404,
                  -81.6557,-82.1115,-80.9270,
                  -124.0346,-156.4448,-77.2975,
                  -156.0218,-156.4410,-101.9896,
                  -156.4450,113.7723,-75.3645,
                  -156.4052,-118.4163,-159.7089,
                  151.7928,-156.4410,115.7518,
                  -174.0187,-80.2298,18.8487,
                  29.5369,136.8667,115.1880,
                  115.5763,152.7350,55.2879,
                  -158.0839,-124.2426,-120.6064,
                  -157.9803,151.7092,-124.0179,
                  151.7092,114.1646,18.5730,
                  -121.1214,28.1189,-122.3928,
                  147.1803,115.3455,135.1582,
                  151.2663,23.3776,-124.0535,
                  115.5073,-121.8093,-159.3687,
                  -155.9969,23.3724,164.2500,
                  115.0392,-77.4316,-82.7145,
                  142.7028,152.7265,-86.7465,
                  115.0920,55.7015,-80.0581,
                  153.3594,114.9858, 115.6638,
                  115.6690,-124.1748,-122.3946,
                  -120.6064,-120.2149,18.4187,
                  153.2052,115.6776,115.6633,
                  -94.7977]})

coordinates_copy = coordinates.copy()
coordinates_copy

df_shark = pd.DataFrame.from_dict(coordinates, orient='index')
df_shark = df_shark.transpose()

pd.set_option('max_columns',None)
pd.set_option('max_rows',None)
df_shark

df_shark.dtypes
df_shark.isnull().sum()
df_shark.shape

df_shark['Latitude'] = df_shark['Latitude'].astype(float)
df_shark['Longitude'] = df_shark['Longitude'].astype(float)
df_shark.dtypes

import geopandas as gpd
pip install descartes

gdf = gpd.GeoDataFrame(
    df_shark, geometry=gpd.points_from_xy(df_shark.Longitude,df_shark.Latitude))
    
gdf.isnull().sum()
gdf.dtypes

#frame_2 = [gdf,shark_df]
#shark_geo = pd.concat(frame_2,sort=True)
#shark_geo
shark_geo = gdf.merge(shark_df)
shark_geo

world = gpd.read_file(gpd.datasets.get_path('naturalearth_lowres'))

country_names = {'Bosnia and Herz.': 'Bosnia and Herzegovina',
                    'Central African Rep.': 'Central African Republic',
                    'Congo': 'Congo (Brazzaville)',
                    'Dem. Rep. Congo': 'Congo (Kinshasa)',
                    'Czechia': 'Czech Republic',
                    "Côte d'Ivoire": 'Ivory Coast',
                    'Dominican Rep.': 'Dominican Republic',
                    'N. Cyprus': 'North Cyprus',
                    'Palestine': 'Palestinian Territories',
                    'Somaliland': 'Somaliland region',
                    'S. Sudan': 'South Sudan',
                    'eSwatini': 'Swaziland', 
                    'Taiwan': 'Taiwan Province of China',
                    'United States of America': 'United States'}

world = world.replace(to_replace=country_names)

print(type(world))

world.head()

fig, ax = plt.subplots(figsize=(30,10))
world.plot(ax=ax, alpha=0.4, color='green', edgecolor='black')
shark_geo.plot('Fatal', categorical=True, cmap='RdBu_r', linewidth=.8, edgecolor='black',
         legend=True, legend_kwds={'bbox_to_anchor':(.2, 1.05),'fontsize':16,'frameon':False}, ax=ax)
ax.axis('off')
ax.set_title('Worldwide Shark Attacks (White, Tiger, Bull) 2010-2020',fontsize=20)
plt.tight_layout()

sns.set(font_scale=1.25)
sns.set_style('darkgrid')
sns.catplot(x='Species',y='Year',data=shark_geo,kind='box')
plt.show()

h = sns.catplot(x='Species',col='State',
           col_wrap=3, data=shark_geo, kind='count',
           height=2.0, aspect=2.5, color='b',
           sharex=False, sharey=False)
h.set_titles('{col_name}')

fig, ax = plt.subplots(figsize=(30,10))
world.plot(ax=ax, alpha=0.5, color='grey', edgecolor='black')
shark_geo.plot(column='Activity', linewidth=.8, edgecolor='black',legend=True,
               legend_kwds={'bbox_to_anchor':(1.5, 1.5),'fontsize':16,'frameon':False}, ax=ax)
plt.title('Shark Attacks by Activities 2010-2020 (White, Tiger, Bull)')

import folium
from shapely.geometry import Point

map = folium.Map(location = [15,30], tiles = "Stamen Terrain", zoom_start = 2)
map

shark_geo_list = [[point.xy[1][0], point.xy[0][0]] for point in shark_geo.geometry ]

i = 0
for coordinates in shark_geo_list:
    if shark_geo.Species[i] == 'White Shark':
        type_color = 'blue'
    elif shark_geo.Species[i] == 'Tiger Shark':
        type_color = 'orange'
    elif shark_geo.Species[i] == 'Bull Shark':
        type_color = 'red'
    
    map.add_child(folium.Marker(location = coordinates,
                            popup =
                            'Year: ' + str(shark_geo.Year[i]) + '<br>' +
                            'Species: ' + str(shark_geo.Species[i]) + '<br>' +
                            'Fatal: ' + str(shark_geo.Fatal[i]) + '<br>' +
                            'Country: ' + str(shark_geo.Country[i]) + '<br>'
                            'Location: ' + str(shark_geo.Location[i]) + '<br>'
                            'Type: ' + str(shark_geo.Type[i]) + '<br>'
                            
                            'Coordinates: ' + str(shark_geo_list[i]),
                            icon = folium.Icon(color = "%s" % type_color)))
    i = i + 1
    
# Here you can zoom in and out of the map to see attack locations and if click 'i' it will provide you the details on the attack
map

from folium import plugins
map = folium.Map(location = [15,30], tiles='Cartodb dark_matter', zoom_start = 2)
heat_data = [[point.xy[1][0], point.xy[0][0]] for point in shark_geo.geometry ]
heat_data
plugins.HeatMap(heat_data).add_to(map)

map

SEED = 100
np.random.seed(SEED)

shark_geo.groupby('Country',axis=0)
shark_geo.set_index('Country',inplace=True)
shark_geo

#shark_geo['Fatal'].replace(to_replace='Y', value=1, inplace=True)
#shark_geo['Fatal'].replace(to_replace='N', value=0, inplace=True)
#shark_geo['Species'].replace(to_replace='White Shark', value=1, inplace=True)
#shark_geo['Species'].replace(to_replace='Tiger Shark', value=2, inplace=True)
#shark_geo['Species'].replace(to_replace='Bull Shark', value=3, inplace=True)
#shark_geo
#shark_geo.sort_values(by=['Year'],ascending=False).reset_index(drop=False)

shark_geo['Type'].unique()
shark_geo['Type'].nunique()
shark_geo['Activity'].unique()
shark_geo['Activity'].nunique()

from sklearn.preprocessing import LabelEncoder
numbers = LabelEncoder()
shark_geo['Fatal'] = numbers.fit_transform(shark_geo['Fatal'].astype('str'))
shark_geo['Type'] = numbers.fit_transform(shark_geo['Type'].astype('str'))
shark_geo['Species'] = numbers.fit_transform(shark_geo['Species'].astype('str'))
#shark_geo['State'] = numbers.fit_transform(shark_geo['State'].astype('str'))
#shark_geo['Location'] = numbers.fit_transform(shark_geo['Location'].astype('str'))
shark_geo['Activity'] = numbers.fit_transform(shark_geo['Activity'].astype('str'))
shark_geo.head(10)

def HeatMap(shark_geo,x=True):
        correlations = shark_geo.corr()
        cmap = 'Reds_r'
        fig, ax = plt.subplots(figsize=(10, 10))
        fig = sns.heatmap(correlations, cmap=cmap, vmax=1.0, center=0, fmt='.2f',
                          square=True, linewidths=.5, annot=x, cbar_kws={"shrink": .75})
        fig.set_xticklabels(fig.get_xticklabels(), rotation = 90, fontsize = 10)
        fig.set_yticklabels(fig.get_yticklabels(), rotation = 0, fontsize = 10)
        plt.tight_layout()
        plt.show()

HeatMap(shark_geo,x=True)

from sklearn.preprocessing import StandardScaler
geo_key_vars = ['Activity','Fatal','Species','Type','Year']
X = shark_geo[geo_key_vars]
Y = shark_geo['Activity']
scaler = StandardScaler().fit(X)
X_scaled = pd.DataFrame(scaler.fit_transform(X), columns = X.columns, index = X.index)

shark_geo['Activity'].nunique()

from sklearn.cluster import KMeans
kcluster = KMeans(n_clusters=65)
kcluster.fit(X_scaled)

from sklearn.model_selection import train_test_split
from sklearn import svm, tree
from sklearn.metrics import accuracy_score
from sklearn.ensemble import  RandomForestRegressor
from sklearn.metrics import mean_squared_error

X_train, X_test, y_train, y_test =train_test_split(X_scaled,Y,
                                                   test_size=0.20,
                                                   random_state=1000)
                                                   
X_train.shape, X_test.shape, y_train.shape, y_test.shape

clf = svm.SVC()
clf_tree = tree.DecisionTreeClassifier(random_state=1000)
for x in range(100):
    X_train,X_test,y_train,y_test= train_test_split(X_scaled,Y)
    clf.fit(X_train,y_train)
    clf_tree.fit(X_train,y_train)
    predictions1 = clf.predict(X_test)
    predictions2 = clf_tree.predict(X_test)
    score1 = accuracy_score(y_test,predictions1)
    score2 = accuracy_score(y_test,predictions2)
    print(score1,score2,score1-score2)
    
kcluster.labels_
sharks_clustered = shark_geo.copy()
sharks_clustered['klabel'] = kcluster.labels_
X_scaled_clustered = X_scaled.copy()
X_scaled_clustered['klabel'] = kcluster.labels_
sharks_clustered

# You can create 65x plots for each activity
def plot_cluster_and_centroid(label):
    X_scaled_clustered[X_scaled_clustered.klabel==label][geo_key_vars].T.plot(legend=True)
    plt.legend(bbox_to_anchor=(1.01, 1),borderaxespad=0)
    plt.plot(kcluster.cluster_centers_[label], 'ko--')
    plt.xticks(range(5), geo_key_vars, rotation=45)

plot_cluster_and_centroid(0)
plot_cluster_and_centroid(1)
plot_cluster_and_centroid(2)
plot_cluster_and_centroid(3)
plot_cluster_and_centroid(4)
plot_cluster_and_centroid(5)

sns.clustermap(X_scaled,
               method ='average',
               metric = 'euclidean',
               vmin=0,vmax=4,
               cmap='mako_r',linewidths=.5,
               figsize = (14,250),annot=True
              )
              
X_train_x = X_train[['Activity','Fatal','Species','Type','Year']]
X_test_x = X_test[['Activity','Fatal','Species','Type','Year']]
rfr = RandomForestRegressor(max_depth=10,random_state=1000)
rfr.fit(X_train_x, y_train)
predict_train = rfr.predict(X_train_x)
predict_test = rfr.predict(X_test_x)
print('RMSE on train data: ', mean_squared_error(y_train, predict_train)**(0.5))
print('RMSE on test data: ',  mean_squared_error(y_test, predict_test)**(0.5))

plt.figure(figsize=(10,7))
feat_importances = pd.Series(rfr.feature_importances_,index=X_train_x.columns)
feat_importances.nlargest(5).plot(kind='line')

x = np.array([[X_scaled['Activity'],X_scaled['Fatal'],
               X_scaled['Species'],X_scaled['Type'],X_scaled['Year']]])

y = np.array(X_scaled['Activity'].values.tolist())
x = x.reshape(x.shape[1:])
x = x.transpose()
x

x.shape
y.shape

from sklearn import linear_model

classifiers = [
    svm.SVR(),
    linear_model.SGDRegressor(),
    linear_model.BayesianRidge(),
    linear_model.LassoLars(),
    linear_model.ARDRegression(),
    linear_model.PassiveAggressiveRegressor(),
    linear_model.TheilSenRegressor(),
    linear_model.LinearRegression()]

for item in classifiers:
    print(item)
    clf = item
    clf.fit(x, y)
    print(clf.predict(x),'\n')
    
from sklearn.datasets import make_blobs
from sklearn.linear_model import LogisticRegression
x, y = make_blobs(n_samples=100, centers=3, n_features=2)
model = LogisticRegression(solver='lbfgs',multi_class='auto')
model.fit(X_train, y_train)
predictions3 = model.predict(X_test)
score3 = accuracy_score(y_test,predictions3)
print(score3)

X_scaled.max()

labels = 'Activity','Fatal','Species','Type','Year'
explode = (0,0.3,0,0,0)
size = [1.21,2.45,0.93,1.62,1.73]
fig1, ax1 = plt.subplots()
ax1.pie(size, explode=explode, labels=labels, autopct='%1.1f%%',
        shadow=True, startangle=90)
ax1.axis('equal')

plt.show()

sns.pairplot(X_scaled,kind='reg',hue='Fatal',palette='ocean')

# Checking Algorithms
def GetBasedModel():
    basedModels = []
    basedModels.append(('LR'   , LogisticRegression()))
    basedModels.append(('LDA'  , LinearDiscriminantAnalysis()))
    basedModels.append(('KNN'  , KNeighborsClassifier()))
    basedModels.append(('CART' , DecisionTreeClassifier()))
    basedModels.append(('NB'   , GaussianNB()))
    basedModels.append(('SVM'  , SVC(probability=True)))
    basedModels.append(('AB'   , AdaBoostClassifier()))
    basedModels.append(('GBM'  , GradientBoostingClassifier()))
    basedModels.append(('RF'   , RandomForestClassifier()))
    basedModels.append(('ET'   , ExtraTreesClassifier()))

    
    return basedModels
    
from sklearn.model_selection import KFold
from sklearn.model_selection import StratifiedKFold
from sklearn.model_selection import cross_val_score
from sklearn.model_selection import GridSearchCV
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.tree import DecisionTreeClassifier
from sklearn.neighbors import KNeighborsClassifier
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis
from sklearn.naive_bayes import GaussianNB
from sklearn.svm import SVC
from sklearn.ensemble import AdaBoostClassifier
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.ensemble import ExtraTreesClassifier

# Test options and evaluation metric
def BasedLine2(X_train, y_train,models):
    num_folds = 10
    scoring = 'accuracy'

    sharks_results = []
    names = []
    for name, model in models:
        kfold = StratifiedKFold(n_splits=num_folds, random_state=SEED)
        sharks_results = cross_val_score(model, X_train, y_train, cv=kfold, scoring=scoring)
        results.append(sharks_results)
        names.append(name)
        msg = "%s: %f (%f)" % (name, sharks_results.mean(), sharks_results.std())
        print(msg)
        
    return names, sharks_results
    
 clf = ExtraTreesClassifier(n_estimators=250,
                              random_state=SEED)

clf.fit(X_train, y_train)

feature_importance = clf.feature_importances_

feature_importance = 100.0 * (feature_importance / feature_importance.max())
sorted_idx = np.argsort(feature_importance)
pos = np.arange(sorted_idx.shape[0]) + .5
plt.subplot(1, 2, 2)
plt.barh(pos, feature_importance[sorted_idx], align='center')
plt.yticks(pos, shark_geo.columns[sorted_idx])
plt.xlabel('Relative Importance')
plt.title('Variable Importance')
plt.show()

from sklearn.model_selection import GridSearchCV, RandomizedSearchCV
from scipy.stats import uniform

class RandomSearch(object):
    
    def __init__(self,X_train,y_train,model,hyperparameters):
        
        self.X_train = X_train
        self.y_train = y_train
        self.model = model
        self.hyperparameters = hyperparameters
        
    def RandomSearch(self):
        cv = 9
        clf = RandomizedSearchCV(self.model,
                                 self.hyperparameters,
                                 random_state=1,
                                 n_iter=100,
                                 cv=cv,
                                 verbose=0,
                                 n_jobs=-1,
                                 )
        
        best_model = clf.fit(self.X_train, self.y_train)
        message = (best_model.best_score_, best_model.best_params_)
        print("Best: %f using %s" % (message))
        return best_model,best_model.best_params_
    
    def BestModelPridict(self,X_test):
        
        best_model,_ = self.RandomSearch()
        pred = best_model.predict(X_test)
        return pred

class GridSearch(object):
    
    def __init__(self,X_train,y_train,model,hyperparameters):
        
        self.X_train = X_train
        self.y_train = y_train
        self.model = model
        self.hyperparameters = hyperparameters
        
    def GridSearch(self):
        cv = 9
        clf = GridSearchCV(self.model,
                                 self.hyperparameters,
                                 cv=cv,
                                 verbose=0,
                                 n_jobs=-1,
                                 )
        
        best_model = clf.fit(self.X_train, self.y_train)
        message = (best_model.best_score_, best_model.best_params_)
        print("Best: %f using %s" % (message))

        return best_model,best_model.best_params_
    
    def BestModelPridict(self,X_test):
        
        best_model,_ = self.GridSearch()
        pred = best_model.predict(X_test)
        return pred
 
model = LogisticRegression()
penalty = ['l1', 'l2']
C = uniform(loc=0, scale=4)
hyperparameters = dict(C=C, penalty=penalty)

LR_RandSearch = RandomSearch(X_train,y_train,model,hyperparameters)
#LR_best_model,LR_best_params = LR_RandSearch.RandomSearch()
Prediction_LR = LR_RandSearch.BestModelPridict(X_test)

sharks_KNN = KNeighborsClassifier()
neighbors = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15]
param_grid = dict(n_neighbors=neighbors)

KNN_GridSearch = GridSearch(X_train,y_train,sharks_KNN,param_grid)
Prediction_KNN = KNN_GridSearch.BestModelPridict(X_test)

def floatingDecimals(f_val, dec=3):
        prc = "{:."+str(dec)+"f}" 
        return float(prc.format(f_val))
        
print('prediction on test set is:' ,floatingDecimals((y_test == Prediction_KNN).mean(),7))

c_values = [0.1, 0.3, 0.5, 0.7, 0.9, 1.0, 1.3, 1.5, 1.7, 2.0]
kernel_values = [ 'linear' , 'poly' , 'rbf' , 'sigmoid' ]
param_grid = dict(C=c_values, kernel=kernel_values)
model_SVC = SVC()

SVC_GridSearch = GridSearch(X_train,y_train,model_SVC,param_grid)
Prediction_SVC = SVC_GridSearch.BestModelPridict(X_test)
print('prediction on test set is:' ,floatingDecimals((y_test == Prediction_SVC).mean(),7))

from scipy.stats import randint
max_depth_value = [3, None]
max_features_value =  randint(1, 4)
min_samples_leaf_value = randint(1, 4)
criterion_value = ["gini", "entropy"]

aram_grid = dict(max_depth = max_depth_value,
                  max_features = max_features_value,
                  min_samples_leaf = min_samples_leaf_value,
                  criterion = criterion_value)
                  
learning_rate_value = [.01,.05,.1,.5,1]
n_estimators_value = [50,100,150,200,250,300]
param_grid = dict(learning_rate=learning_rate_value, n_estimators=n_estimators_value)

sharks_Ad = AdaBoostClassifier()
Ad_GridSearch = GridSearch(X_train,y_train,sharks_Ad,param_grid)
Prediction_Ad = Ad_GridSearch.BestModelPridict(X_test)
print('prediction on test set is:' ,floatingDecimals((y_test == Prediction_Ad).mean(),7))

learning_rate_value = [.01,.05,.1,.5,1]
n_estimators_value = [50,100,150,200,250,300]
param_grid = dict(learning_rate=learning_rate_value, n_estimators=n_estimators_value)

sharks_GB = GradientBoostingClassifier()
GB_GridSearch = GridSearch(X_train,y_train,sharks_GB,param_grid)
Prediction_GB = GB_GridSearch.BestModelPridict(X_test)
print('prediction on test set is:' ,floatingDecimals((y_test == Prediction_GB).mean(),7))

def get_models():
    """Generate a library of base learners."""
    param = {'C': 0.7678243129497218, 'penalty': 'l2'}
    model1 = LogisticRegression(**param)

    param = {'n_neighbors': 8}
    model2 = KNeighborsClassifier(**param)

    param = {'C': 1.7, 'kernel': 'linear', 'probability':True}
    model3 = SVC(**param)

    param = {'criterion': 'gini', 'max_depth': 3, 'max_features': 2, 'min_samples_leaf': 3}
    model4 = DecisionTreeClassifier(**param)

    param = {'learning_rate': 0.05, 'n_estimators': 150}
    model5 = AdaBoostClassifier(**param)

    param = {'learning_rate': 0.01, 'n_estimators': 100}
    model6 = GradientBoostingClassifier(**param)

    model7 = RandomForestClassifier()

    model8 = ExtraTreesClassifier()

    models = {'LR':model1, 'KNN':model2, 'SVC':model3, 'DT':model4,
              'ADa':model5, 'GB':model6, 'RF':model7,  'ET':model8
              }
    return models
    
 def train_predict(model_list,X_train, X_test, y_train, y_test):
    """Fit models in list on training set and return preds"""
    P = np.zeros((y_test.shape[0], len(model_list)))
    P = pd.DataFrame(P)

    print("Fitting models.")
    cols = list()
    for i, (name, m) in enumerate(models.items()):
        print("%s..." % name, end=" ", flush=False)
        m.fit(X_train, y_train)
        P.iloc[:, i] = m.predict_proba(X_test)[:, 1]
        cols.append(name)
        print("done")

    P.columns = cols
    print("Done.\n")
    return P
    
models = get_models()
P = train_predict(models,X_train,X_test,y_train,y_test)

corrMatrix = X_scaled.corr()
print (corrMatrix)

ax = sns.heatmap(
    corrMatrix, 
    vmin=-1, vmax=1, center=0,
    cmap=sns.diverging_palette(20, 220, n=200),
    square=True
)
ax.set_xticklabels(
    ax.get_xticklabels(),
    rotation=45,
    horizontalalignment='right'
)

from string import ascii_letters
mask = np.triu(np.ones_like(corrMatrix, dtype=bool))
f, ax = plt.subplots(figsize=(11, 9))
cmap = sns.diverging_palette(220,20, as_cmap=True)

sns.heatmap(corrMatrix, mask=mask, cmap=cmap, vmax=.3, center=0,
            square=True, linewidths=.5, cbar_kws={"shrink": .5})
            
pip install mlens
from mlens.visualization import corrmat
corrmat(P.corr(), inflate=False)
