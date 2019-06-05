# ccs_build
attach ccs names to icd10 conditions using sas and python




dfDXMLABEL10 = pd.read_csv(r'src\ccs\MultiCCSLoadPrograms\DXMLABEL10.CSV', dtype=str)
dfPRMLABEL10 = pd.read_csv(r'src\ccs\MultiCCSLoadPrograms\PRMLABEL10.CSV', dtype=str)
dfccs_dx_icd10 = pd.read_csv(r'src\ccs\MultiCCSLoadPrograms\ccs_dx_icd10cm_2019_1(1)\ccs_dx_icd10cm_2019_1.csv', dtype=str)

dfccs_dx_icd10.columns = [i[1:][:-1] for i in dfccs_dx_icd10.columns]

dfccs_dx_icd10[['ICD-10-CM CODE', 'CCS CATEGORY', 'MULTI CCS LVL 1', 'MULTI CCS LVL 2']] =\
    dfccs_dx_icd10[['ICD-10-CM CODE', 'CCS CATEGORY', 'MULTI CCS LVL 1', 'MULTI CCS LVL 2']].apply(lambda c: c.str[1:-1]).copy()

dfccs_dx_icd10['ICD-10-CM CODE'] = dfccs_dx_icd10['ICD-10-CM CODE'].str.strip().copy()

dfccsMapping = pd.merge(pd.melt(dfMed[['Dx1', 'Dx2', 'Dx3']].reset_index(), id_vars='index', value_vars=['Dx1', 'Dx2', 'Dx3']),
dfccs_dx_icd10,
left_on='value', right_on='ICD-10-CM CODE')
