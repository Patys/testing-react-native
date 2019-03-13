# Testing React Native

It's a list of snippets, that will help you test your component properly.

## Snapshot

Simple test to check changes in snapshots. Pretty useful, but not really testing logic, but just props and style. Easy to ignore.

It should be only your base. You will start here and add more tests to it.

```javascript
describe('Component: EXAMPLECOMPONENT', () => {
  describe('with basic props', () => {
    const props = {
      ...defaultProps
    }

    const wrapper = shallow(
      <EXAMPLECOMPONENT {...props} />
    )

    it('Should render snapshot properly', () => {
      expect(wrapper).toMatchSnapshot()
    })
  })
})
```

## Testing components

To make sure some components are on screen you can easily add test to find it.

#### Count number of elements
```javascript
it('Should render properly', () => {
  expect(wrapper.find(View).length).toBe(1)
})
```

#### Find specific element
```javascript
<View testID="COMPONENT_TO_TEST">
```

```javascript
it('Should has component', () => {
  const component = wrapper.findWhere(node => node.prop('testID') === 'COMPONENT_TO_TEST')
  expect(component).toBeDefined()
})
```

## Test styles

Sometimes we need to has some specific styles, e.g. mandatory style for button.

```javascript
it('Should has mandatory style', () => {
  const component = wrapper.findWhere(node => node.prop('testID') === 'COMPONENT_TO_TEST')
  expect(component.props().style).toHaveProperty('color', 'red')
})
```


```javascript
it('Should has mandatory style', () => {
  const expectedStyle = {
    color: 'red'
  }
  const component = wrapper.findWhere(node => node.prop('testID') === 'COMPONENT_TO_TEST')
  expect(component.props().style).toMatch(expectedStyle)
})
```

## Test component logic

Sometimes we have some helpers functions inside our components, e.g. getIcon, getValue or anything.
```javascript
import { icons } from '../assets'
```

```javascript
it('Should return default icon', () => {
  const icon = wrapper.instance().getIcon()
  expect(icon).toEqual(icons.default)
})
```

```javascript
it('Should return 42', () => {
  wrapper.instance().makeCalculation()
  wrapper.update()
  const value = wrapper.instance().getValue()
  expect(value).toEqual(42)
})
```

## Test buttons

Sometimes we pass function through props like this:
```javascript
  <MyComponent onPress={() => amazing()} />
```

and inside MyComponent we use it like this to call this `amazing` function

```javascript
  <TouchableOpacity onPress={() => this.props.onPress()}>
```

So we can check does this prop was called and how many times

```javascript
it('Should handle item click', () => {
  wrapper.find(TouchableOpacity).simulate('press')
  expect(props.onPress).toHaveBeenCalledTimes(1)
})
```
