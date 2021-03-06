﻿@page  "/focusZonePage"

<header class="root">
    <h1 class="title">FocusZone</h1>
</header>
<div class="section" style="transition-delay: 0s;">
    <div id="overview" tabindex="-1">
        <h2 class="subHeading hiddenContent">Overview</h2>
    </div>
    <div class="content">
        <div class="ms-Markdown">
            <p>
                FocusZones abstract arrow key navigation behaviors. Tabbable elements (buttons, anchors, and elements with data-is-focusable='true' attributes) are considered when pressing directional arrow keys and focus is moved appropriately. Tabbing to a zone sets focus only to the current "active" element, making it simple to use the tab key to transition from one zone to the next, rather than through every focusable element.
            </p>
            <p>
                Using a FocusZone is simple. Just wrap a bunch of content inside of a FocusZone, and arrows and tabbling will be handled for you! See examples below.
            </p>
        </div>
    </div>
</div>
<div class="section" style="transition-delay: 0s;">
    <div id="overview" tabindex="-1">
        <h2 class="subHeading">Usage</h2>
    </div>
    <div>
        <div class="subSection">

            <h4>FocusZone with horizontal movement</h4>

            <div style="padding: 50px; background-color:yellow;">
                <FocusZone Direction="FocusZoneDirection.Horizontal">
                    <DefaultButton Text="First Button" OnClick=@OnInnerClick />
                    <DefaultButton Text="Second Button" OnClick=@OnInnerClick />
                    <DefaultButton Text="Third Button" OnClick=@OnInnerClick />
                </FocusZone>
            </div>
        </div>
        <div class="subSection">
            <DefaultButton Text="Outside Button" OnClick=@OnOuterClick />

            <h4>FocusZone with vertical &amp; circular movement</h4>

            <FocusZone Direction="FocusZoneDirection.Vertical" IsCircularNavigation="true">
                <List ItemsSource=@items>
                    <ItemTemplate>
                        <div style="display:flex;flex-direction:row;width:100%;" data-is-focusable="true">
                            <Image Src="redArrow.jpg" Height="50" Width="50" />
                            <Label>This is an item #@context</Label>
                        </div>
                    </ItemTemplate>
                </List>
            </FocusZone>
        </div>
        <div class="subSection">
            <style>
    .photoCell {
        position: relative;
        display: inline-block;
        padding: 2px;
        box-sizing: border-box;
    }

        .photoCell:focus {
            outline: none;
        }

            .photoCell:focus:after {
                content: "";
                position: absolute;
                right: 4px;
                left: 4px;
                top: 4px;
                bottom: 4px;
                border: 1px solid @(Theme.Palette.White);
                outline: 2px solid @(Theme.Palette.ThemePrimary);
            }
            </style>

            <h4>FocusZone with bidirectional movement</h4>
            <FocusZone Direction="FocusZoneDirection.Bidirectional"
                       Style="display:inline-block;border:1px solid var(--palette-NeutralTertiary);padding:10px;line-height:0;overflow:hidden;">
                @for (var i = 0; i < photos.Count; i++)
                {
                    <li @key=@i
                        data-is-focusable="true"
                        class="photoCell">
                        <Image Src=@photos[i].Url
                               Width=@photos[i].Width
                               Height=@photos[i].Height />
                    </li>
                }
            </FocusZone>
        </div>
    </div>
</div>
@code {
    //ToDo: Add Demo sections
    bool isFocusTrapped = false;
    string debugText = "";

    [Inject] public ThemeProvider ThemeProvider { get; set; }

    public ITheme Theme => ThemeProvider.Theme;

    System.Collections.Generic.List<string> items = new System.Collections.Generic.List<string>();

    class Photo
    {
        public string Url { get; set; }
        public double Height { get; set; }
        public double Width { get; set; }
    }

    System.Collections.Generic.List<Photo> photos = new System.Collections.Generic.List<Photo>();

    override protected Task OnInitializedAsync()
    {
        for (int i = 0; i < 6; i++)
        {
            items.Add(i.ToString());
        }

        var random = new Random();

        for (int i = 0; i < 20; i++)
        {
            var randomWidth = 50 + Math.Floor(random.NextDouble() * 150);
            photos.Add(new Photo() { Url = $"http://placehold.it/{randomWidth}x100", Height = 100, Width = randomWidth });
        }
        return Task.CompletedTask;
    }

    Task OnInnerClick(MouseEventArgs args)
    {
        return Task.CompletedTask;
    }
    Task OnOuterClick(MouseEventArgs args)
    {
        return Task.CompletedTask;
    }
}
