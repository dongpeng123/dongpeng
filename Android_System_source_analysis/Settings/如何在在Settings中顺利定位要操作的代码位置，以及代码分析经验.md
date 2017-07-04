# 如何在在Settings中顺利定位要操作的代码位置，以及代码分析经验
内容：

- 如何根据页面相关信息定位到具体代码
- 如何由代码定位到问题所在

## １.如何根据页面相关信息定位到具体代码
- 首相，我们明确了页面主界面的入口点，即xml里的dashboard_categories.xml文件，由此可以找到对应点击item所启动的fragment(Settings)
的目前实现，大部分仍然是由fragment组成，部分是独立activity。
- 但当我们界面不确定的时候该如何，首先根据页面样式中出现的文字，通过在String.xml中定位检索，找到相应的id,然后根据相应的id逆向查找
到使用此id的布局，然后根据布局结构，定位到具体的代码位置
- 如果界面嵌套太深，一方面可以从最上层一层层深入，也可以直接ctrl+shift+r 整体搜索，找到所有相关文件，在在里边摘出有用信息
- 小细节，在fragment中使用当前上下文的方式通过getActivity()的方式拿到
- 在preference布局中，为起设置监听的具体实现也有所不同，需要重写onPreferenceClick（）相关方法，若有处理，返回真
- 若你实现的功能条目，嵌套多层fragment,如何使其能够记忆当前是由哪些入口点击进入的而不会造成混乱，需要特殊关注（具体实现细节具体分析）：
```
 public static final Indexable.SearchIndexProvider SEARCH_INDEX_DATA_PROVIDER =
            new BaseSearchIndexProvider() {
                @Override
                public List<SearchIndexableResource> getXmlResourcesToIndex(Context context,
                        boolean enabled) {
                    ArrayList<SearchIndexableResource> result =
                            new ArrayList<SearchIndexableResource>();

                    SearchIndexableResource sir = new SearchIndexableResource(context);
                    sir.xmlResId = R.xml.display_settings;
                    result.add(sir);

                    return result;
                }

                @Override
                public List<String> getNonIndexableKeys(Context context) {
                    ArrayList<String> result = new ArrayList<String>();
                    if (!context.getResources().getBoolean(
                            com.android.internal.R.bool.config_dreamsSupported)) {
                        result.add(KEY_SCREEN_SAVER);
                    }
                    if (!isAutomaticBrightnessAvailable(context.getResources())) {
                        result.add(KEY_AUTO_BRIGHTNESS);
                    }
                    if (!isLiftToWakeAvailable(context)) {
                        result.add(KEY_LIFT_TO_WAKE);
                    }
                    if (!isDozeAvailable(context)) {
                        result.add(KEY_DOZE);
                    }
                    if (!RotationPolicy.isRotationLockToggleVisible(context)) {
                        result.add(KEY_AUTO_ROTATE);
                    }
                    return result;
                }
            };
``` 

## ２.如何由代码定位到问题所在
- 有时候即使定位到了具体代码位置，仍然会有分析以及执行过程中的多种问题，如何解决
- 首先，若你的执行结果出现，且错误，那应该是你的实现有问题；重点说一下出现crash的情形如何处理，我们在界面上看不到任何信息
- 出现这种情况，我们只有通过log的方式区看，找到崩溃的地方
　若是要简介对应，我们可以通过启动模拟器（保证你的模拟器能够复现此crash），然后通过<br />
　　　　然后，启动服务器：adb devices<br />
　　　　准备开启取log模式：adb logcat > issue.log<br />
　　　　最后在另一个窗口：tail -f issue.log (即可看到log信息)<br />
　的方式拿出所有log信息，然后检索crash，找到崩溃点，进行分析；若是使用真机测试，直接把真机信息取出即可<br />
　常见crash(空指针异常，数组越界)
- 对于更细化的功能实现条目的问题，需要怎么分析处理，只能在合适的位置打log去分析处理

