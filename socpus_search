# -*- coding: utf-8 -*-
"""
@author: LuisMDlab & eduardo2s
"""
#In construction#

#%%IMPORTATIONS
import pandas as pd
from scopus import ScopusSearch
from scopus import ScopusAbstract
from scopus import ScopusAuthor
import pickle
results = {}
eids = {}
results_dict = {}

document = pd.read_csv('your_file.csv')

#%%Search a csv of terms
for binomio in document['Nomes']:
    try:
        print(binomio)
        results[binomio] = ScopusSearch(binomio, max_entries=60000, refresh=True)
    except Exception as e:
        print(e)
        continue
    
#Take just Eids of the results.
for binomio in document['Nomes']:
    print(binomio)
    if binomio in results:
        if len(results[binomio].EIDS) > 0:
            eids[binomio] = results[binomio].EIDS
            
pickle.dump(eids, open( "eids.p", "wb" )) #Save the eids dict.

#%%Process Data

#N. of publications
soma = 0
for binomio in eids.keys():
    soma += len(eids[binomio])
print(soma)

#Take eids objetcts and turn into ScopusAbstract objects
article_info = {}
for binomio in eids.keys():
    print(binomio)
    article_info[binomio] = []
    for eid in eids[binomio]:
        print(eid,  end=',')
        try:
            article_info[binomio].append(ScopusAbstract(eid))
        except Exception as e:
            print(e)
            continue

pickle.dump(eids, open("article_info.p","wb")) #Save the article_info dict.


#Turn the autors data in a list
def get_authors(obj_list):
    author_list = []
    for au_obj in obj_list:
        author_list.append(ScopusAuthor(au_obj.auid).name)
    return ",".join(author_list)
#%% Article Info Extraction

#N. of articles by searched terms
for i in article_info.keys():
    print(i, len(article_info[i]))
    
#Process atributes of ScopusAbstract object from the articles
for binomio in article_info.keys():
    print(binomio)
    contagem = 0
    for article in article_info[binomio]:
        contagem += 1
        if article == 'Nulo':
            continue
        else:
            try:
                #Returns the searched Term
                results_dict.setdefault('binomio', ['Nulo'])
                results_dict['binomio'].append(binomio)
                
                #Returns arcticle's DOI
                results_dict.setdefault('doi', ['Nulo'])
                results_dict['doi'].append(article.doi)
                
                #Retruns article's Title
                results_dict.setdefault('titulo', ['Nulo'])
                results_dict['titulo'].append(article.title)
                
                #Returns article's Abstract
                results_dict.setdefault('abstract', ['Nulo'])
                results_dict['abstract'].append(article.abstract)
    
                #Returns Author's Name
                results_dict.setdefault('autor', ['Nulo'])
                results_dict['autor'].append(get_authors(article.authors))
    
                #Return article's Scopus Link
                results_dict.setdefault('link', ['Nulo'])
                results_dict['link'].append(article.url)
    
                #Returns Journal's ISSN
                results_dict.setdefault('issn', ['Nulo'])
                results_dict['issn'].append(article.issn)
    
                #Returns Journal's Name
                results_dict.setdefault('nomePeriodico', ['Nulo'])
                results_dict['nomePeriodico'].append(article.publicationName)
    
                #Returns Publication Date
                results_dict.setdefault('data', ['Nulo'])
                results_dict['data'].append(article.coverDate)
                
                #Returns Author's Number
                results_dict.setdefault('n_autores', ['Nulo'])
                results_dict['n_autores'].append(article.nauthors)
                
                #Return References Number
                results_dict.setdefault('n_ref', ['Nulo'])
                results_dict['n_ref'].append(article.refcount)
                print(str(contagem/len(article_info[binomio])*100)+'%')
                
            except Exception as e:
                #Returns the searched Term
                results_dict.setdefault('binomio', ['Nulo'])
                results_dict['binomio'].append('Nulo')
                
                #Returns arcticle's DOI
                results_dict.setdefault('doi', ['Nulo'])
                results_dict['doi'].append('Nulo')
                
                #Retruns article's Title
                results_dict.setdefault('titulo', ['Nulo'])
                results_dict['titulo'].append('Nulo')
                
                #Returns article's Abstract
                results_dict.setdefault('abstract', ['Nulo'])
                results_dict['abstract'].append('Nulo')
    
                #Returns Author's Name
                results_dict.setdefault('autor', ['Nulo'])
                results_dict['autor'].append('Nulo')
    
               #Return article's Scopus Link
                results_dict.setdefault('link', ['Nulo'])
                results_dict['link'].append('Nulo')
    
                #Returns Journal's ISSN
                results_dict.setdefault('issn', ['Nulo'])
                results_dict['issn'].append('Nulo')
    
                #Returns Journal's Name
                results_dict.setdefault('nomePeriodico', ['Nulo'])
                results_dict['nomePeriodico'].append('Nulo')
    
                #Returns Publication Date
                results_dict.setdefault('data', ['Nulo'])
                results_dict['data'].append('Nulo')
                
                Returns Author's Number
                results_dict.setdefault('n_autores', ['Nulo'])
                results_dict['n_autores'].append('Nulo')
                
                #Return References Number
                results_dict.setdefault('n_ref', ['Nulo'])
                results_dict['n_ref'].append('Nulo')
                print(str(contagem/len(article_info[binomio])*100)+'%')
                continue
                
#%% Results to CSV
#Deal with the arrays of dict:
df_results = pd.DataFrame.from_dict(results_dict, orient='index')
df_results = df_results.transpose()

df_results.to_csv('resultados_binomios.csv', encoding='utf-8')
