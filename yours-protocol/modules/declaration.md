# 📚 Declaration

Yours Protocol provides metadata for explorers and users which modules are supported in each dapp. In order to make your module show up in explorers you have to extend the following function.

```kotlin
@extend(yours.module_metadata)
function module_metadata() {
  return [
    yours.module_info(
      name = "characters",
      version = 1,
      description = "Defines how a demo character looks like"
    )
  ]
}
```

This is fully optional but it is recommended if you want to provide information about your module and make it re-usable.
