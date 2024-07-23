
What is amazon locker?  
When a customer orders items with amazon they may choose to deliver it to locker which customer can pick it up on the way to their home.

Customers can also return items to lockers which can be picked up pickup team than pickup form home geoLocation.

Locker Sizes

1. Small
2. Medium
3. Large
4. Extra Large
5. Double Extra Large

Customer while ordering item can pick up their nearest geoLocation of locker where items are shipped to.  
Items are kept for maximum of 3 days.  
User gets locker code using which they can open the locker.  
Once the locker is closed the code is expired and cannot be open again.

1. Assign a closest locker to a person given current co-ordinates( where customer wants )
2. After order is delivered by courier service to customer specified amazon locker, a 6 digit code will be sent to customer .
3. Item will be placed in Amazon locker for 3 days
4. After 3 days, if Item is not picked up from the locker, refund process will be initiated
5. Amazon locker will be assigned to customer based on the size of the locker ( S,M,L,XL,XXL)
6. Only items that are eligible to be put in locker can be assigned to amazon locker .i.e Not all items can be placed inside locker (like furniture can't be put inside amazon locker)
7. Access to Amazon locker will depend on Store's opening and closing time.(Since Amazon locker are placed inside stores,malls etc)
8. Items can be returned to Amazon locker as well .
9. Items that are eligible Amazon locker item, can only be returned by customer
10. Once the Door is closed. User's code will be expired. (User will not be able to open the locker now)

### Model:

enum **LockerSize**{
S,M,L,XL,XXL
}

enum **LockerState**{
	OPEN,CLOSED,AVAILABLE,BOOKED
}

**Item**{
	name
	type
}

**Package**{
	 String orderId
	 double size
	 list<Item> items
}

**Order**{
	String id,
	List<Item>
	GeoLocation deloverylocation
}

**Locker**{
	id,
	LockerSize;
	LockerState status;
	
}

**LockerTiming**{
	Map<DayOfWeek, Timing> timingMap;
}

**LockerLocation**{
	list<Locker>
	id
	GeoLocation location
	LockerTiming
}

**Notification**{
	orderId,
	packegeId,
	lockerCode
	lockerId
	lockerLocationId
}

**LockerPackage**{
	validAge =3days
	lockerId
	orderId
	lockerCode
	packageDeliveredDate

boolean verifyCode(String code);
boolean isValidPackage(){
	if(currentTime-packageDeliveredDate > validAge)
		return false;
		return true;
	}
}



### Services

```
public class **LockerService** {

    public Locker **findLockerIbyId**(String id) {
        return LockerRepository.lockerMap.get(id);
    }

    public Locker **getLocker**(LockerSize lockerSize, GeoLocation geoLocation) {
        return getAvailableLocker(lockerSize, geoLocation);
    }

    public void **pickFromLocker**(String lockerId,
                               String code, LocalDateTime localDateTime) throws
            LockerNotFoundException, LockeCodeMisMatchException, PackPickTimeExceededException,
            PickupCodeExpiredException {
        Optional<LockerPackage> lockerPackage =
                LockerPackageRepository.getLockerByLockerId(lockerId);
        if (!lockerPackage.isPresent())
            throw new LockerNotFoundException("Locker with code not found");
        if (!lockerPackage.get().verifyCode(code))
            throw new LockeCodeMisMatchException("Locker code mismatch");
        if (!lockerPackage.get().isValidCode(localDateTime)) {
            throw new PickupCodeExpiredException("Pickup code expired");
        }
        Locker locker = LockerRepository.lockerMap.get(lockerId);
        if (canPickFromLocker(lockerId, localDateTime)) {
            locker.setLockerStatus(LockerStatus.AVAILALBE);
            lockerPackage.get().setCode(null);
        } else {
            lockerPackage.get().setCode(null);
            throw new PackPickTimeExceededException("Package not picked for x days");
        }
    }

    private Locker **getAvailableLocker**(LockerSize lockerSize,
                                      GeoLocation geoLocation) {
        return **checkAndGetAvailableLockers**(lockerSize, geoLocation);
    }

    private Locker **checkAndGetAvailableLockers**(LockerSize lockerSize,
                                               GeoLocation geoLocation) {
        Locker locker = LockerRepository.getLocker(lockerSize, geoLocation);
        locker.setLockerStatus(LockerStatus.BOOKED);
        return locker;
    }

    private boolean **canPickFromLocker**(String lockerId, LocalDateTime localDateTime) {
        Locker locker = LockerRepository.lockerMap.get(lockerId);
        LockerLocation lockerLocation = LockerLocationRepository.getLockerLocation(locker.getLocationId());
        LocationTiming locationTiming = lockerLocation.getLocationTiming();
        Timing timing = locationTiming.getTimingMap().get(localDateTime.getDayOfWeek());
        Time currentTime = Time.valueOf(getTimeFromDate(localDateTime));
        if (currentTime.after(timing.getOpenTime()) && currentTime.before(timing.getCloseTime())) {
            return true;
        }
        return false;
    }

    private String **getTimeFromDate**(LocalDateTime localDateTime) {
        SimpleDateFormat localDateFormat = new SimpleDateFormat("HH:mm:ss");
        String time = localDateFormat.format(new Date());
        return time;
    }
}
```
```public class **DeliveryService** {
    NotificationService notificationService = new NotificationService();
    OrderService orderService = new OrderService();
    LockerService lockerService = new LockerService();

    public void deliver(String orderId) throws PackageSizeMappingException {
        Order order = orderService.getOrder(orderId);
        List<Item> items = orderService.getItemsForOrder(orderId);
        Pack pack = new Pack(orderId, items);
        LockerSize lockerSize = SizeUtil.getLockerSizeForPack(pack.getPackageSize());
        Locker locker = lockerService.getLocker(lockerSize, order.getDeliveryGeoLocation());
        LockerPackage lockerPackage = new LockerPackage();
        lockerPackage.setOrderId(orderId);
        lockerPackage.setLockerId(locker.getId());
        lockerPackage.setCode(IdGenerator.generateId(6));
        LockerPackageRepository.lockerPackages.add(lockerPackage);
        locker.setLockerStatus(LockerStatus.CLOSED);
        notificationService.notifyCustomerOrder(lockerPackage);
    }
}
```

