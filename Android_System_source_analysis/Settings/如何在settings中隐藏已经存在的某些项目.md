
# 如何在settings中隐藏已经存在的某些项目

settings所有项目的布局加载文件为xml/dash_category.xml，如果想要隐藏某些模块不是简单的在xml/dash_category.xml里删掉或注释掉就可以了，因为在某些情况下，万一这个模块，在其他代码中使用了其中的id,当注释或删除此块代码，牵一发而动全身，所以要从整个模块加载流程入手：

 - 首先通过一个方法加载并部署该dashboradcagegory
    
    SettingsActivity.java
    
        categories.clear();
        loadCategoriesFromResource(R.xml.dashboard_categories, categories);
        updateTilesList(categories);
    }
    
  - 先调用loadCagegoriesFromResource加载该布局,然后有个updateTilesList(categories)的过程，接下来我们关注这个点
  
             final boolean showDev = mDevelopmentPreferences.getBoolean(
                 DevelopmentSettings.PREF_SHOW,
                 android.os.Build.TYPE.equals("eng"));

             final UserManager um = (UserManager) getSystemService(Context.USER_SERVICE);

             final int size = target.size();
             for (int i = 0; i < size; i++) {

                 DashboardCategory category = target.get(i);

                 // Ids are integers, so downcasting is ok
                 int id = (int) category.id;
                 int n = category.getTilesCount() - 1;
                 while (n >= 0) {

                     DashboardTile tile = category.getTile(n);
                     boolean removeTile = false;
                     id = (int) tile.id;
                     if (id == R.id.operator_settings || id == R.id.manufacturer_settings) {
                         if (!Utils.updateTileToSpecificActivityFromMetaDataOrRemove(this, tile)) {
                             removeTile = true;
                         }
                     } else if (id == R.id.wifi_settings) {
                         // Remove WiFi Settings if WiFi service is not available.
                         if (!getPackageManager().hasSystemFeature(PackageManager.FEATURE_WIFI)) {
                             removeTile = true;
                         }
                     } else if (id == R.id.bluetooth_settings) {
                         // Remove Bluetooth Settings if Bluetooth service is not available.
                         if (!getPackageManager().hasSystemFeature(PackageManager.FEATURE_BLUETOOTH)) {
                             removeTile = true;
                         }
                     } else if (id == R.id.data_usage_settings) {
                         // Remove data usage when kernel module not enabled
                         final INetworkManagementService netManager = INetworkManagementService.Stub
                                 .asInterface(ServiceManager.getService(Context.NETWORKMANAGEMENT_SERVICE));
         .............................

        } else if (id == R.id.privacy_settings) {
                         removeTile = true;
                     } else if (id == R.id.apps_compatibility_settings) {
                         removeTile = true;
                     } else if (id == R.id.jabol_settings) {
                         removeTile = true;
                         
        里边有个判断，如果是这个Id,我们对removeTile有个赋值操作，为true则表示在Settings的布局不再显示该条目，达到隐藏的目的。
