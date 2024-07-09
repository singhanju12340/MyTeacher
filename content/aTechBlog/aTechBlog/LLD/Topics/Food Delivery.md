**Requirements:**

User should be able to see all the menu
user should be able to book order of selected menu
user should be able to see old order history
admin user should be able to do CRUD with menu items
User can track order delivery person
user can get delivery contact person info
User can add and update item in cart
user can fetch cart items
USer can apply coupans for discount
user can see bills for given order with tax calculation
Food prepration states should be visible to users
delivery boy can get list of items delivered by him
Restaurants can register on delivery app
user can search for near by restaurant or by name
user can filter food menu based on Meal type and cuisine type



**Services:**
ResturentService
UserService
LocationSearch
DeliveryService

```
class ResturentService{
	private List<Resturent> resturents;
	public List<Resturent> getAllResturents();
	public void addResturents(@NotNull final Resturent resturent);
	public Restautant searchRestaurantsByName(@NonNull final String name);
	public Restautant searchRestaurantsByCity@NonNull final String name);
}
```

```
UserService{
	private List<User> users;
	public void getAllUser();
	public User getUser();
	public User UpdateUser();
	
}
```


```class FoodMenuService{
	private List<Item> items;

	public List<Item> fetchResturentMenu(String restaurantId);
	public List<Item> fetchResturentItemByFilter(String restaurantId, String menuType);
	public List<Item> fetchByCuisineType(String restaurantId, String cuisine);
	public Item getMenuById(String itemId);
	public void addItemInRestaurants(Item item);
}
```


We can use the **Command Pattern** to handle the add or remove commands.
**Customers** can add/remove items — **updateCart**

**Customers** can empty the cart — **clearCart**

**Customers** can see the cart items — **getAllItemsOfCart**
```
class CartService{
CartService(CartData, FoodMenuService, CartUpdateCommand);
	public deleteCart(userId, resturantId);

	public getAllCart(@NonNull final String userID, @NonNull final String restautantId);
public void updateCart(itemId, userId, resturantId, CartUpdateCommand);


}
```


```
PricingService{
//Based on coupan code we can decide discount patterns
	public Bill getBilll(userId, restaurantId, CoupanCode);
	
	
}
```


```
OrderService{
	public Order getOrderDetail(String orderId);
	public void updateOrderId(resturentId, String status);
	public List<Order> getAllOrderByREsturants(resturantId);
	public List<Order> getAllOrderByREsturantsAndDateRange(resturantId, startTime, endTime);
	getAllOrders(String userId);
}
```