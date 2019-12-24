---
title: "PermissionsDispatcher 4.x: A bunch of stuff"
date: 2018-12-25
draft: false
tags: ["android", "oss"]
---

From July of this year we’ve been adding/removing a bunch of stuff to/from PermissionsDispatcher 4.x(the latest is 4.2.0 for now). I’m personally honored that there’re no changes which break backward API compatibility at all, but some of the changes are not trivial and supposed to be recognized to let you build a reliable and substantial runtime permissions grant flow in your application. In this article I’ll go through some significant changelog topics respectively in minutes.

### AndroidX support

*PR: [https://github.com/permissions-dispatcher/PermissionsDispatcher/pull/494](https://github.com/permissions-dispatcher/PermissionsDispatcher/pull/494)*

Most importantly we’ve started off supporting AndroidX in favor of Android [Jetpack](https://developer.android.com/jetpack/) components advent. Hence to upgrade PermissionsDispatcher to 4.x from previous versions you have to complete AndroidX migration in your application beforehand.

### Drop android.app.Fragment support

*PR: [https://github.com/permissions-dispatcher/PermissionsDispatcher/pull/498](https://github.com/permissions-dispatcher/PermissionsDispatcher/pull/498)*

Related to the AndroidX support mentioned above we simultaneously dropped android.app.Fragment support as it has been deprecated.

### Drop Xiaomi support

*Issue: [https://github.com/permissions-dispatcher/PermissionsDispatcher/issues/544](https://github.com/permissions-dispatcher/PermissionsDispatcher/issues/544)*

Xiaomi is the thing that we’ve been seriously struggling with for years. Originally we decided to add a dedicated workaround for Xiaomi since they customized Android framework permission mechanism and the presented code on [Android Developers](https://developer.android.com/training/permissions/requesting) didn’t work well as expected. But [reportedly](https://github.com/permissions-dispatcher/PermissionsDispatcher/issues/533) recent Miui(OS on Xiaomi) has revised their customization and we don’t need to puzzle over it anymore! We strongly believe that our decision is for the right direction but also recommend you to test your app behavior on several physical Xiaomi devices if a percentage of the device is unignorable. Note that this change is from **4.1.0**.

### Incremental annotation processing

*Issue: [https://github.com/permissions-dispatcher/PermissionsDispatcher/issues/473](https://github.com/permissions-dispatcher/PermissionsDispatcher/issues/473)*

Gradle 4.7 introduced incremental annotation processing and seemingly some popular libraries have started supporting the feature like Dagger2 has done. Likewise PermissionsDispatcher supports the incremental processing to make build time faster from **4.2.0**! Although be sure to note that the feature is **only for Java** because kapt hasn’t supported incremental annotation processors yet. For more detail check YouTrack link below.

[*https://youtrack.jetbrains.com/issue/KT-23880](https://youtrack.jetbrains.com/issue/KT-23880)*

### Flexible OnShowRationale handling

*Issue: [https://github.com/permissions-dispatcher/PermissionsDispatcher/issues/434](https://github.com/permissions-dispatcher/PermissionsDispatcher/issues/434)*

We’ve got a feedback that current OnShowRationale annotated method signature is sort of hard to handle especially with DialogFragment since parameter’s type PermissionRequest is not serializable. After a few discussions we decided to change API as below(don’t worry we keep a backward compatibility) from **4.2.0**.

* Make PermissionRequest parameter optional

* When OnShowRationale annotated method has no parameter processor will generate process${NeedsPermissionMethodName}PermissionRequest and cancel${NeedsPermissionMethodName}PermissionRequest at compile time

Consequently if you want to show DialogFragment in OnShowRationale annotated method, it can be done easily! Here’s a minimum example.

    @RuntimePermissions
    class MainActivity : AppCompatActivity() {
        @NeedsPermission(Manifest.permission.CAMERA)
        fun showCamera() {
        }

        @OnShowRationale(Manifest.permission.CAMERA)
        fun showRationaleForCamera() {
          PermissionDialogFragment().show()
        }
    }

Then processor will generate processShowCameraPermissionRequest and cancelShowCameraPermissionRequest as extension methods. Utilize them as appropriate in DialogFragment.

    class PermissionDialogFragment : DialogFragment() {
      private lateinit var activity: MainActivity

      override fun onAttach(activity: Activity) {
        super.onAttach(activity)
        if (activity is MainActivity) {
          this.activity = activity
        }
      }

      override fun onCreateDialog(savedInstanceState: Bundle?): Dialog {
        return AlertDialog.Builder(activity)
                 .setPositiveButton("OK") {_, _ ->
                   activity.processShowCameraPermissionRequest()
                 }
                 .setNegativeButton("NO") {_, _ ->
                   activity.cancelShowCameraPermissionRequest()
                 }
                 .create()
      }
    }

### New maven groupId

*Issue: [https://github.com/permissions-dispatcher/PermissionsDispatcher/issues/560](https://github.com/permissions-dispatcher/PermissionsDispatcher/issues/560)*

Lastly, library’s artifacts groupId has been changed from com.github.hotchemi to org.permissionsdispatcher from **4.2.0**. The reason of the change is we’ve had issues in the past that people accidentally pulled in from Jitpack, not JCenter that we provide our artifacts to. The problem behind it is Jitpack will pick up on any group ID starting with com.github. This would mean PermissionsDispatcher users need to specify jcenter() first, which contains a potential risk to encounter [malicious dependencies](https://blog.autsoft.hu/a-confusing-dependency/) incident. This change is merely cumbersome for existing users but we’re recognizing that preventing any security risks is the best thing to be prioritized.

### Others

* We prepared a cozy document website upon GitBook! [https://permissions-dispatcher.github.io](https://permissions-dispatcher.github.io/)

* Several [bug fixes](https://permissions-dispatcher.github.io/CHANGELOG.html) are included as usual.

It’s actually kind of incredible that we’ve been maintaining PermissionsDispatcher over 3 years and it’s used in plenty of applications across the world! I can assure we’re going to continuously push so hard to make the library better and provide pleasant developer experience to users from now on.
