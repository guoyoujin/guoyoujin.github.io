title: fragment onAttach(Context context) and onAttach(Activity activity)
author: TryCatch
tags:
  - android
categories:
  - android
date: 2017-05-17 14:42:00
---
在使用dagger2为了解决依赖关系注入fragment的时候，发现我在注入Presenter的时候老是报空指针异常，郁闷了两天没解决这个问题，代码如下

抽象framgent
```
public abstract class BaseFragment extends Fragment {
    private static final String TAG = "BaseFragment";

    @Override
    public void onAttach(Context context) {
        super.onAttach(context);
        setupComponent((TxAssistantComponent) TxAssistantApplication.get(context).component());
    }
    protected abstract void setupComponent(TxAssistantComponent appComponent);
}
```

在需要的fragment里面注入
```
@Inject
 OrderListFragmentPresenter orderListFragmentPresenter;

 @Override
    protected void setupComponent(TxAssistantComponent appComponent) {
        DaggerOrderListFragmentComponent.builder().txAssistantComponent(appComponent)
                .orderListFragmentModule(new OrderListFragmentModule())
                .build().inject(this);
    }
```


这个写法在android5.0以上是没问题的，后来回到家写代码的时候用了一个android4.4的手机，就出现上面所说的错误，这就把我郁闷的了，搞了一晚上没搞定，总感觉代码写的没错，由于对dagger2没啥了解，我就以为是dagger2的注入可能出了问题，于是就各种搜索，最后只能先睡觉了。

今天在公司发现用android5.0的手机就是运行流畅，脑子灵光一闪，会不会是生命周期的问题，果断serch了一下，还真是这个问题，导致这个根本的问题就是国内的android系统被深度定制引起的，后面找到解决办法，只需在全局抽象fragment添加如下方法即可
```
public abstract class BaseFragment extends Fragment {
    private static final String TAG = "BaseFragment";

    @Override
    public void onAttach(Context context) {
        super.onAttach(context);
        setupComponent((TxAssistantComponent) TxAssistantApplication.get(context).component());
    }

    @Override
    public void onAttach(Activity activity) {
        super.onAttach(activity);
        setupComponent((TxAssistantComponent) TxAssistantApplication.get(activity).component());

    }

    protected abstract void setupComponent(TxAssistantComponent appComponent);
}
```