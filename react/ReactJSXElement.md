### ReactJSXElement

#### 核心代码
##### createElement
``` js
export function createElement(type, config, children) {
  let propName;

  const props = {};

  let key = null;
  let ref = null;

  if (config != null) {
    if (hasValidRef(config)) {
      if (!enableRefAsProp) {
        ref = config.ref;
        if (!disableStringRefs) {
          ref = coerceStringRef(ref, getOwner(), type);
        }
      }
    }
    if (hasValidKey(config)) {
      key = '' + config.key;
    }

    for (propName in config) {
      if (
        hasOwnProperty.call(config, propName) &&
        propName !== 'key' &&
        (enableRefAsProp || propName !== 'ref') &&
        propName !== '__self' &&
        propName !== '__source'
      ) {
        if (enableRefAsProp && !disableStringRefs && propName === 'ref') {
          props.ref = coerceStringRef(config[propName], getOwner(), type);
        } else {
          props[propName] = config[propName];
        }
      }
    }
  }

  const childrenLength = arguments.length - 2;
  if (childrenLength === 1) {
    props.children = children;
  } else if (childrenLength > 1) {
    const childArray = Array(childrenLength);
    for (let i = 0; i < childrenLength; i++) {
      childArray[i] = arguments[i + 2];
    }
    props.children = childArray;
  }

  // Resolve default props
  if (type && type.defaultProps) {
    const defaultProps = type.defaultProps;
    for (propName in defaultProps) {
      if (props[propName] === undefined) {
        props[propName] = defaultProps[propName];
      }
    }
  }
  
  return ReactElement(
    type,
    key,
    ref,
    undefined,
    undefined,
    getOwner(),
    props,
    __DEV__ && enableOwnerStacks ? Error('react-stack-top-frame') : undefined,
    __DEV__ && enableOwnerStacks ? createTask(getTaskName(type)) : undefined,
  );
}
```