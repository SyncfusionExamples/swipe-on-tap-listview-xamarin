# How to swipe the Xamarin.Forms ListView item while tapping (SfListView)

The Xamarin.Forms [SfListView](https://help.syncfusion.com/xamarin/listview/overview) allows you to swipe the [ListViewItem](https://help.syncfusion.com/cr/xamarin/Syncfusion.ListView.XForms.ListViewItem.html) in [ItemTapped](https://help.syncfusion.com/cr/xamarin/Syncfusion.ListView.XForms.SfListView.html#Syncfusion_ListView_XForms_SfListView_ItemTapped) event. When swiping the item in **ItemTapped** event, the touch will handle the selection if Selection is enabled for SfListView and the next touch will be handled for the swipe. Also, you can achieve this requirement by skipping the selection as mentioned in the following three ways.

You can also refer the following article.

https://www.syncfusion.com/kb/11925/how-to-swipe-the-xamarin-forms-listview-item-while-tapping-sflistview

**Method 1:**

**XAML**

Disable selection for **SfListView** by using [SelectionMode ](https://help.syncfusion.com/cr/xamarin/Syncfusion.ListView.XForms.SfListView.html#Syncfusion_ListView_XForms_SfListView_SelectionMode)as [**None**](https://help.syncfusion.com/cr/xamarin/Syncfusion.ListView.XForms.SelectionMode.html#Syncfusion_ListView_XForms_SelectionMode_None). In this case, when tapping on one item will swipe the item and tapping another item will reset the swiped item.

``` xml
<syncfusion:SfListView x:Name="listView" SelectionMode="None" ItemSize="60" AllowSwiping="True" ItemsSource="{Binding ContactsInfo}">
    <syncfusion:SfListView.ItemTemplate >
        <DataTemplate>
            <Grid x:Name="grid">
            ...
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.ItemTemplate>
    <syncfusion:SfListView.LeftSwipeTemplate>
        <DataTemplate>
            <Grid BackgroundColor="Teal">
                <Label Text="Left swipe template" HorizontalOptions="Center" VerticalOptions="Center" LineBreakMode="NoWrap"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.LeftSwipeTemplate>
</syncfusion:SfListView>
```

**C#**

In the **ItemTapped** event, swipe the **ListViewItem** using [SfListView.SwipeItem](https://help.syncfusion.com/cr/xamarin/Syncfusion.ListView.XForms.SfListView.html#Syncfusion_ListView_XForms_SfListView_SwipeItem_System_Object_System_Double_) method.

``` c#
namespace ListViewXamarin
{
    public class Behavior : Behavior<ContentPage>
    {
        SfListView ListView;
        protected override void OnAttachedTo(ContentPage bindable)
        {
            ListView = bindable.FindByName<SfListView>("listView");
            ListView.ItemTapped += ListView_ItemTapped;
            base.OnAttachedTo(bindable);
        }

        private void ListView_ItemTapped(object sender, Syncfusion.ListView.XForms.ItemTappedEventArgs e)
        {
            ListView.SwipeItem(e.ItemData, ListView.SwipeOffset);
        }
    }
}
```
**Method 2:**

**C#**

Set [Handled](https://help.syncfusion.com/cr/xamarin/Syncfusion.ListView.XForms.ItemTappedEventArgs.html#Syncfusion_ListView_XForms_ItemTappedEventArgs_Handled)property of the [ItemTappedEventArgs](https://help.syncfusion.com/cr/xamarin/Syncfusion.ListView.XForms.ItemTappedEventArgs.html) as  **true**  to handle the touch by **ItemTapped** event. In this case, the touch will not be passed to process the selection. Also, tapping another item will reset the swiped item.

``` c#
namespace ListViewXamarin
{
    public class Behavior : Behavior<ContentPage>
    {
        SfListView ListView;
        protected override void OnAttachedTo(ContentPage bindable)
        {
            ListView = bindable.FindByName<SfListView>("listView");
            ListView.ItemTapped += ListView_ItemTapped;
            base.OnAttachedTo(bindable);
        }

        private void ListView_ItemTapped(object sender, Syncfusion.ListView.XForms.ItemTappedEventArgs e)
        {
            ListView.SwipeItem(e.ItemData, 200);
            e.Handled = true;
        }
    }
}

```

**Method 3:**

**XAML**

You can swipe the ListViewItem using [**GestureRecongnizers**](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/gestures/tap) for [ItemTemplate](https://help.syncfusion.com/cr/xamarin/Syncfusion.ListView.XForms.SfListView.html#Syncfusion_ListView_XForms_SfListView_ItemTemplate). In this case, touch will be handled by the gestures instead of ListViewItem. So, tapped item will be swiped on each interaction.

``` xml
<syncfusion:SfListView x:Name="listView" SwipeOffset="200" ItemSize="60" AllowSwiping="True" ItemsSource="{Binding ContactsInfo}">
    <syncfusion:SfListView.ItemTemplate >
        <DataTemplate>
            <Grid x:Name="grid">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Grid.GestureRecognizers>
                    <TapGestureRecognizer Tapped="TapGestureRecognizer_Tapped"/>
                </Grid.GestureRecognizers>
                <Image Source="{Binding ContactImage}" VerticalOptions="Center" HorizontalOptions="Center" HeightRequest="50" WidthRequest="50"/>
                <Grid Grid.Column="1" RowSpacing="1" Padding="10,0,0,0" VerticalOptions="Center">
                    <Label LineBreakMode="NoWrap" TextColor="#474747" Text="{Binding ContactName}"/>
                    <Label Grid.Row="1" Grid.Column="0" TextColor="#474747" LineBreakMode="NoWrap" Text="{Binding ContactNumber}"/>
                </Grid>
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.ItemTemplate>
    <syncfusion:SfListView.LeftSwipeTemplate>
        <DataTemplate>
            <Grid BackgroundColor="Teal">
                <Label Text="Left swipe template" HorizontalOptions="Center" VerticalOptions="Center" LineBreakMode="NoWrap"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.LeftSwipeTemplate>
</syncfusion:SfListView>
```
**C#**

``` c#
namespace ListViewXamarin
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }

        private void TapGestureRecognizer_Tapped(object sender, EventArgs e)
        {
            var currentItem = (sender as Grid).BindingContext as Contacts;
            listView.SwipeItem(currentItem, listView.SwipeOffset);
        }
    }
}
```

**Output**

![SwipeOnTap](https://github.com/SyncfusionExamples/swipe-on-tap-listview-xamarin/blob/master/ScreenShot/SwipeOnTap.gif)
