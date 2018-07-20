参考:https://www.kaggle.com/startupsci/titanic-data-science-solutions



按类别特征分组求Survived rate:

```python
train_df[['Sex', 'Survived']].groupby(['Sex']).mean().sort_values(by='Survived')
```



