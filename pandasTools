# -*- coding: utf-8 -*-
import pandas as pd
from collections import defaultdict
from copy import deepcopy

def series2dict(srs: pd.Series):
    d = defaultdict(lambda: 'N/A')
    return srs.to_dict(d)
    
    
def transpose(df):
    try:
        return pd.DataFrame(df.values.T, index=df.columns, columns=df.index)
    except:
        return df.stack().unstack(0)
    
    
def dict2df(d, index):
    return pd.DataFrame(d, index = [index])
    
    
def series2df(srs, index=None):
    if index:
        return transpose(srs.rename(index).to_frame())
    else:
        return transpose(srs.to_frame())
    # return srs.rename(index).to_frame().stack().unstack(0)

    
def dict2series(d, name=None):
    return pd.Series(d, name=name)
    

def updateDataFrame(srs, df, main_key = None, index = None):
    '''
    srs: 字典或Series，将要并入DataFrame的有序数组
    df：目标DataFrame
    main_key: 主键
    index: 如果指定，则主键被覆盖
    有返回值。原dataframe不会被更新
    '''
    try:
        if index is None:
            index = srs[main_key]
        if isinstance(srs, dict):   
            new_df = pd.DataFrame(srs, index = [index])
        elif isinstance(srs, pd.Series):
            new_df = series2df(srs, index = index)
        if (df.index == index).any():       # 判断是否存在
            df = deepcopy(df)               # 深度copy，防止原数据被覆盖
            df.update(new_df)               # 如果存在，则更新本条
        else:
            df = df.append(new_df)          # 如果不存在，则在末尾续加一条
    except KeyError:
        pass
    finally:
        return df
    
if __name__ == '__main__':
    import numpy as np
    df = pd.DataFrame(np.arange(12).reshape(3, 4))
    s1 = pd.Series(np.random.rand(4), index=[0, 1, 2, 3])
    # print(df)
    srs = pd.Series(df.loc[1,:].to_dict())
    print(series2df(srs, srs.name))
