# Bitcoin transaction optimization

## Implementation
### Fee prediction

1. To collect the data we are forked nnfe repo and hosting the our version [here](https://gitlab.inyar.ru/bitcoin-transaction-optimization/nnfe).
We can collect the data from past starting approximately from 2016 (which is limiteed by mempool statistics).
    * Please find more details in [Collector doc](Collector.md)
2. Then data processed and prediction model learned by code in fee predictor [repo](https://gitlab.inyar.ru/bitcoin-transaction-optimization/predictor-trainer)
    * Please find more details in [Fee predictor trainer doc](FeePredictorTrainer.md)
3. After that the model is used by fee predictor api which based on suggested transaction characteristics, looks for mempool characteristics and responds with prediction result.
    * Please check out the [Predictor service doc](FeePredictorService.md) for more details.

### Pay request generator
The application generates test pay requests and pushes them into pay-request SQS. These pay requests are consumed by transaction optimizer.

### Transaction optimizer
The application pulls pay requests from pay-request queue and assembles transactions. Prepared transactions are pushed into prepared-transactions SQS.

### Transaction sender
The app responsible for pulling transactions from prepared-transactions SQS and sending them into bitcoin network. More details [here](TransactionSender.md).
