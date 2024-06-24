**Eviction strategies**
1. Least Recently Used (Jump to head on access) 
2. Least Frequently Used 
3. FIFO 
4. LIFO
5. A hybrid algorithm

##### **Requirement:**
store different data sets
fetch data by key
remove value from cache by TTL
remove value from cache based on eviction policies

##### **NFR:**
concurrency
latency
search in O(1)
save in O(1)

**What data structure and algo we can use**
doubly linked list + Hashmap and use evicition policy as LRU
![[cache-dig|4000]]
> call for get(3)
	    then tail will contain 3, as tail is keeping the least recently used
> call to add new element 10, but size of the cache is 4 only, and its full.
> 	10 will be set as tail and head will be shifted to next position
> 	new entry will be added in hashmap, and old head entry will be removed from map
> call to remove (2)
> 	remove 2 node from DDL
> 	remove 2 from hashmap

##### Sudocode:

```
```public class Cache{

		private CacheStorage storage;
		private EvictionPolicy ePolicy;
		
		cache(EvictionPolicy ePolicy, CacheStorage storage )
		{
			this.storage = storage;
			this.ePolicy = ePolicy;
		}
		
		public String get(String key)
		{
			this.ePolicy.keyAccessed(key);
			return this.storage.get(key);
			// fetch from storage map
			// call keyAccessed
		}
		
		public void put(String key, String value){
			this.storage.put(key, value);
			
			// check if key not present in storage map
			// check if size is not reached, else call evict()
			// create DDL node, insert in map
			// move it to tail using keyAccesssed()
			
		}
		
}

public Interface EvictionPolicy{
	keyAccessed(String key);
	evict();
}

public Interface CacheStorage {

		public String get(String key);
		public void put(String key, String value);
}

public DDLNode{
	String next;
	String prev;
	String data;
}

public DDL{
	private DDLNode head;
	private DDLNode tail;
	public DDLNode getHeadNode();
	public DDLNode getTailNode();
	public DDLNode getNodeAtEnd();
	public DDLNode inserthNodeAtEnd(key);
	public void DetatchNode(DDLNode node);
}



public class LRUEvictionPolicy{
	private Map<String,DDLNode> evictionNodeMap;
	private DDL ddl;

	keyAccessed(String key){
		// move key node to tail
		if(evictionNodeMap.containsKey(key)){
			ddl.detatchNode(evictionNodeMap.get(key));
			ddl.attachAtEnd(evictionNodeMap.get(key));
		}else{
			DDLNode currentNode = ddl.attachAtEnd(key);
			evictionNodeMap.put(key, currentNode);
		}
	}
	public String evict(){
		// remove head element
			DDLNode head = ddl.getFirstNode();
			if(head == null)
				return;
			else{
			ddl.remove(head);
			evictionNodeMap.remove(head.data);
			return head.data;
			}
			
			
	}
}

public class MapCacheStorage {
		private int size;
		private Map<String, String> storage;
		
		public String get(String key){
			if(!storage.contains(key)){
				return null;
			}else
				return storage.get(key);

		}
		
		public void put(String key, String value){
			if(!isFull())
				storage.put(key, value);
		}
		
		MapCacheStorage(int size){
			this.size;
		}
		
		public String remove(String key){
			if(!storage.contains(key)){
				// throw key not found;
			}else
			return storage.get(key);
		}
		
		private boolean isFull(){
			return storage.size()==size;
		}
		
}

psvm(){
	Cache cache = new Cache(new MapCacheStorage(10), new LRUEvictionPolicy());
	 cache.put();
	 cache.put();
	 cache.put();
	 cache.remove();
	 cache.get(key);
}









