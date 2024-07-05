# Popover Window

One way to create a window that appears when a user clicks on something is to use the Popover component from the next-org/react library.

The main components involved are:

- Popover - the main window container
- PopoverTrigger - a wrapper around the element that when clicked triggers the window to be displayed
- PopoverContent - the element that contains the content to be displayed in the window

Example:

```typescript
<Popover placement="left">
  <PopoverTrigger>
    <Avatar src={session.user.image || ''} />
  </PopoverTrigger>
  <PopoverContent>
    <div className="p-4">
      <form action={actions.signOut}>
        <Button type="submit">Sign Out</Button>
      </form>
    </div>
  </PopoverContent>
</Popover>
```

In the above the Popover placement property is set to 'left' meaning that the window will be displayed to the left of the element that is clicked. The PopoverTrigger wraps the Avatar element which is what the user will click to cause the window to be displayed. The PopoverContent contains the contents of the window when it is displayed, which in this case is a button attached to a form for signing out of the application.
