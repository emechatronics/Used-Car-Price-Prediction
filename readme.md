# predicting-the-costs-of-used-cars

![](Snapshots/working_app.gif)

<center><i>working App developed using Flask</i></center><br>

Dataset used here is from a hackathon hosted by MachineHack. Go to the hackathon homepage to know more about the dataset. The dataset set contains features like Location, Manufacture details, car features such as Fuel type, Engine, and usage parameters. Below is the app in Working condition.

## Best Model

### Pipeline Config 

``` python
pipeline = Pipeline([
    ('features',DFFeatureUnion([
        ('numerics', Pipeline([
            ('extract',ColumnExtractor(con_cols)),
            ('log', Log1pTransformer()),
            ('col_Interact',DFadd_ColInteraction('Kilometers_Driven','Mileage'))
        ])),
        ('nominal_OneHot',Pipeline([
            ('extract',ColumnExtractor(One_hot_cols)),
            ('dummy',DummyTransformer())])),
        ('nominal_Target', Pipeline([
            ('extract',ColumnExtractor(Tar_cols)),
            ('Mean_Enc',TargetEncoder())])),
        ('Year',Pipeline([
            ('extract',ColumnExtractor(Year)),
            ('Shift',ShiftTranformer(par=2019))])),
        ('Seats',Pipeline([
            ('extract',ColumnExtractor(Seats)),
            ('Select_OneHot',DF_OneHotEncoder(filter_threshold=0.05))]))
        ])),
    ('Model_fit',GradientBoostingRegressor())])

pipe_params= {
    'Model_fit__n_estimators': [10,50,100,150,200,250,500,750],
    'Model_fit__learning_rate': [0.01,0.1,0.5,1],
    'Model_fit__subsample': [0.1,0.2,0.5,1.0],
}
```

### Best parametes

``` python
{'Model_fit__learning_rate': 0.1,
  'Model_fit__n_estimators': 500,
  'Model_fit__subsample': 0.5}
```

### Validiton plot:

![](Snapshots/Best_model_validRes.png)


### Export whole Model Pipeline

``` python
with open('gbm_model_dump.pkl', 'wb') as f:
    pickle.dump(model, f, 2)
```