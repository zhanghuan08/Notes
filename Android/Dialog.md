# Dialog

## Dialog设置

```java
//设置ip的对话框
private void showSetIpDialog() {
    AlertDialog.Builder builder = new AlertDialog.Builder(getContext());
    builder.setView(R.layout.dialog_ip);
    //点击对话框之外不允许关闭
    builder.setCancelable(false);

    AlertDialog dialog = builder.create();
    dialog.show();
    EditText mEtIp = dialog.findViewById(R.id.et_ip);
    //获取value
    Editable ip = mEtIp.getText();
    //点击了取消按钮
    dialog.findViewById(R.id.btn_cancel).setOnClickListener(v -> dialog.dismiss());
    //点击了确定按钮
    dialog.findViewById(R.id.btn_true).setOnClickListener(v -> {
    //校验IP正则
String regex = "";
regex = "^((2(5[0-5]|[0-4]\\d))|[0-1]?\\d{1,2})(\\.((2(5[0-5]|[0-4]\\d))|[0-1]?\\d{1,2})){3}$";
        Pattern pattern = Pattern.compile(regex);
        Matcher matcher = pattern.matcher(ip);
        boolean bool = matcher.matches();
        if (bool) {
            //如果验证通过
            Toast.makeText(getActivity(), "输入的IP为:"+ip, Toast.LENGTH_SHORT).show();
            dialog.dismiss();
        } else {
            //如果验证通过
            Toast.makeText(getActivity(), "IP格式错误", Toast.LENGTH_SHORT).show();
        }
    });
    Window window = dialog.getWindow();
    if (window != null)
        window.setBackgroundDrawableResource(android.R.color.transparent);
}
```

## XML

```xml
// 设置IP输入 为单行输入
 android:singleLine="true"
// 最大行数
 android:maxLines="1"
// 输入类型
 android:inputType="number"
// 属性限定输入的字符
 android:digits="0123456789."
```

