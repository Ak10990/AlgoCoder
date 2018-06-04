## Understand the Testing Pyramid

### Small tests 
Unit tests that you can run in isolation from production systems.
They typically mock every major component and should run quickly on your machine.
    ### Automated unit tests:
        ### Local tests:
            Unit tests that run on your local machine only.
            These tests are compiled to run locally on the Java Virtual Machine (JVM) to minimize execution time.
            Use this approach to run unit tests that have no dependencies on the Android framework
            or have dependencies that can be filled by using mock objects.
        ### Instrumented tests: 
            Unit tests that run on an Android device or emulator.
            These tests have access to instrumentation information, such as the Context for the app under test.
            Use this approach to run unit tests that have Android dependencies which cannot be easily
            filled by using mock objects.


### Medium tests 
Integration tests that sit in between small tests and large tests.
They integrate several components, and they run on emulators or real devices.
Examples of medium tests include service tests, integration tests, and hermetic UI tests
that simulate the behavior of external dependencies

### Large tests 
Integration and UI tests that run by completing a UI workflow.
They ensure that key end-user tasks work as expected on emulators or real devices.

UI Automator testing (https://developer.android.com/training/testing/ui-automator.html)
https://firebase.google.com/docs/test-lab/
instrumented unit tests
Roboelectric , Mockito(https://github.com/mockito/mockito)


## JUnit
## JUnit4 rules with Android Test
### ActivityTestRule
This rule provides functional testing of a single activity. 
The activity under test will be launched before each test annotated with @Test and before any method annotated with @Before.
It will be terminated after the test is completed and all methods annotated with @After are finished.
The activity under test can be accessed during your test by calling ActivityTestRule.getActivity().

@RunWith(AndroidJUnit4.class)
@LargeTest
public class MyClassTest {
    @Rule
    public ActivityTestRule<MyClass> mActivityRule =
            new ActivityTestRule(MyClass.class);

    @Test
    public void myClassMethod_ReturnsTrue() { ... }
}

### ServiceTestRule
This rule provides a simplified mechanism to start up and shut down your service before and after the duration of your test. 
It also guarantees that the service is successfully connected when starting a service or binding to one. 
The service can be started or bound using one of the helper methods. 
It will automatically be stopped or unbound after the test completes and any methods annotated with @After are finished.
@RunWith(AndroidJUnit4.class)
@MediumTest
public class MyServiceTest {
    @Rule
    public final ServiceTestRule mServiceRule = new ServiceTestRule();

    @Test
    public void testWithStartedService() {
        mServiceRule.startService(
            new Intent(InstrumentationRegistry.getTargetContext(),
            MyService.class));

        // Add your test code here.
    }

    @Test
    public void testWithBoundService() {
        IBinder binder = mServiceRule.bindService(
            new Intent(InstrumentationRegistry.getTargetContext(),
            MyService.class));
        MyService service = ((MyService.LocalBinder) binder).getService();
        assertTrue("Service didn't return true",
            service.doSomethingToReturnTrue());
    }
}

## Note
If the exceptions thrown are problematic for your tests, 
you can change the behavior so that methods instead return either null or zero 
by adding the following configuration in your project's top-level build.gradle file:
android {
  ...
  testOptions {
    unitTests.returnDefaultValues = true
  }
}

## Other References
https://junit.org/junit4/javadoc/latest/org/junit/Assert.html
https://github.com/hamcrest



