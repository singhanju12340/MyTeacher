---
Creation Time: Tuesday, July 2nd 2024
Modified Time: Monday, January 6th 2025
---

Stock trading can work on publisher/observer patterns

StockMarket company can be publisher
End user or any other company can be observer


```
public interface Publisher{
	addObserver()
	unRegisterObserver()
	updateStockValue(val)
	notifyLatestPriceToAllObserver()
}


public interface Observer{
	display()
	unSubscribe()
	update(newValue)
}



StockMarket Class which implements the Publisher interface:

class NiftyStockMarket implements Publisher{
	Set<Observer> observers;
	int value;
	addObserver(Observer newObserver){
		observers.add(newObserver);
	}

	unRegisterObserver(Observer newObserver){
		observers.remove(newObserver);
	}
	
	updateStockValue(Integer newValue){
		this.value = newValue;
	}

	notifyLatestPriceToAllObserver(){
		foreach(Observer obs : observers){
			obs.update(this.value);
		}
	}

}


class PayPayStock implementation Observer{
	int latestValue;
	Publisher publisher;

	PayPayStock(Publisher stockMarket){
		publisher.registerObserver(stockMarket);
		latestValue = 0;
		publisher = stockMarket;
		
	}
	public update(int newValue){
		latestValue = newValue;
		display();
	}
	
	public void display(){
		sout(latestValue);
	}

	public void unSubscribe(){
		publisher.unRegisterObserver(this);
	}

	

}




