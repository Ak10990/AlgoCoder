# Android Architecture

## MVVM(Data Binding)

### Integrating
// <app-module>/build.gradle
apply plugin: "com.android.application"
android { dataBinding {
        enabled = true
      }
}

<data class="CustomClassName">
<variable name="user" type="com.example.User"/>
</data>
 android:text="@{user.firstName}"
 MainActivityBinding binding =
.setContentView(this, R.layout.main_activity); binding.myId.setText("John Doe")

### Usages
Use for any activity
DataBindingUtil.setContentView(activity, layoutId);
Use for any view
DataBindingUtil.inflate(inflater, layoutId, parent, attachToParrent); 
Bind view that was already inflated
ListItemBinding binding = ListItemBinding.bind(viewRoot);

Binding Adapters
<layout> <ImageView
      bind:imageUrl="@{viewModel.someNiceImageUrl}"
      bind:error="@{@drawable/defaultImage}"/>
</layout>
@BindingAdapter({"bind:imageUrl", "bind:error"})
public static void loadImage(ImageView view, String url, Drawable error)
{
Picasso.with(view.getContext()).load(url).error(error).into(view);
}
@BindingAdapter({"bind:imageUrl", "bind:error"})
public static void loadImage(ImageView view, String url, Drawable error) {
Picasso.with(view.getContext()).load(url).error(error).into(view);
 }
 
### Notifying view
Notifying view #1
public class User extends BaseObservable { private String firstName;
   public User(String firstName) {
       this.firstName = firstName;
}
public void setFirstName(String firstName) {
       this.firstName = firstName;
   }
   public String getFirstName() {
       return this.firstName;
} }
Notifying view #2
public class User {
public final ObservableField<String> firstName;
public User(String name) {
this.firstName = new ObservableField<>(firstName);
}
public void setName(String firstName) { this.firstName.set(firstName);
} }

### Binding expression operators
Mathematical + - / * % 
String concatenation + 
Logical && ||
Binary & | ^
Unary + - ! ~
Shift >> >>> << 
Comparison == > < >= <=
instanceof, cast
Grouping ()
Literals character, String, numeric, null
Method calls, field access 
Ternary operator ?:
Array access []
Null coalescing operator ??{
            android:text="@{user.displayName != null ? user.displayName : user.lastName}"
                                      =
             android:text="@{user.displayName ?? user.lastName}"
             }
             

### Observable fields & collections
Observable<T> 
ObservableBoolean 
ObservableByte 
ObservableChar 
ObservableShort 
ObservableInt 
ObservableLong 
ObservableFloat 
ObservableDouble 
ObservableParcelable<T>
ObservableList<T> 
ObservableArrayList<T> 
ObservableMap<K, V> 
ObservableArrayMap<K, V>

### java.lang.IllegalStateException
Required DataBindingComponent is null.
If you don't use an inflation method taking a DataBindingComponent, 
use DataBindingUtil.setDefaultComponent or make all BindingAdapter methods static.

Binding Component
public class MyDataBindingCoponent implements DataBindingComponent {
public EditTextBindings getEditTextBindings() {
return new EditTextBindings(); }
}

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
binding = DataBindingUtil .setContentView(this,
R.layout.activity_main,
new MyDataBindingCoponent());
    binding.setViewModel(new ViewModel());
}

### Binding Conversions
android:background="@{isError ? @color/red : @color/white}"
@BindingConversion
public static ColorDrawable convertColorToDrawable(int color) {
   return new ColorDrawable(color);
}

### Binding methods
@BindingMethods({
@BindingMethod(type = CircularProgressBar.class,
attribute = "progressText", method = "setProgressMessage")
})
public class ViewBindings
{
}

### https://github.com/radzio/android-data-binding-recyclerview
  <android.support.v7.widget.RecyclerView
        android:id="@+id/activity_users_recycler"
        app:itemViewBinder="@{view.itemViewBinder}"
        app:items="@{usersViewModel.users}"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:scrollbars="vertical" />
        @Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState); 
usersViewModel = new UsersViewModel();
usersViewModel.users
               .add(new SuperUserViewModel(new User("Android", "Dev")));
binding = DataBindingUtil.setContentView(this, R.layout.users_view); 
binding.setUsersViewModel(usersViewModel);
binding.setView(this);
}

public class UsersViewModel extends BaseObservable {
@Bindable
public ObservableArrayList<UserViewModel> users;
public UsersViewModel() {
        this.users = new ObservableArrayList<>();
    }
public void addUser(String name, String surname) {
this.users.add(new UserViewModel(new User(name, surname))); }
}

public ItemBinder<UserViewModel> itemViewBinder() {
return new CompositeItemBinder<UserViewModel>(
new SuperUserBinder(BR.user, R.layout.item_super_user), new UserBinder(BR.user, R.layout.item_user)
); }

### Lambdas
<Button
    android:onClick="@{() -> viewModel.sendAction()}
/>
 public class MainViewModel extends BaseObservable
{
    public void sendAction()
    {
//code
} }

<Button
    android:onClick="@{viewModel::sendAction}"
/>
 public class MainViewModel extends BaseObservable
{
    public void sendAction(View view)
    {
//code
} }

### Two-Way data binding
android:text="@={viewModel.twoWayText}"

Views with Two-Way Binding support
• AbsListView -> android:selectedItemPosition
• CalendarView -> android:date
• CompoundButton -> android:checked
• DatePicker -> android:year, android:month, android:day
• NumberPicker -> android:value
• RadioGroup -> android:checkedButton
• RatingBar -> android:rating
• SeekBar -> android:progres
• TabHost -> android:currentTab
• TextView -> android:text
• TimePicker -> android:hour, android:minute

### Custom views
@InverseBindingAdapter(attribute = "color", event = "colorAttrChanged") public static int getColor(ColorPickerView view)
{
    return view.getColor();
}

@BindingAdapter("colorAttrChanged")
public static void setColorListener(ColorPickerView view, final InverseBindingListener colorChange)
{
if (colorChange == null) {
        view.setOnColorChangedListener(null);
    }
else
{
view.setOnColorChangedListener(new OnColorChangedListener() {
@Override
            public void onColorChanged(int newColor)
            {
                colorChange.onChange();
            }
}); }
}

@BindingAdapter("color")
public static void setColor(ColorPickerView view, int color) {
     if (color != view.getColor())//Avoiding cycles
    {
        view.setColor(color);
    }
}

public class TwoWayViewModel extends BaseObservable
{
    private int color;
    public void setColor(int color)
    {
        this.color = color;
this.notifyPropertyChanged(BR.color); }
@Bindable
public int getColor() {
        return this.color;
    }
}

public class TwoWayBindingActivity extends AppCompatActivity {
    private ActivityTwoWayBinding binding;
@Override
protected void onCreate(@Nullable Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
binding = DataBindingUtil.setContentView(this, R.layout.activity_two_way); binding.setViewModel(new TwoWayViewModel());
} }

<LinearLayout>
<com.github.danielnilsson9.colorpickerview.view.ColorPickerView
        bind:color="@={viewModel.color}"
        />
<EditText
android:text="@={viewModel.color}" />
<Button
android:text="Set defined color"
android:onClick="@{() -> viewModel.setColor(Color.parseColor(`#C97249`))}" />
</LinearLayout>


### How it works?
 Bind ViewModel to View 
 Register for property change callbacks
 Trigger notifyPropertyChanged(User input or from code)
 Request rebind
 Execute pending bindings
 
