---
id: 39
title: Python code to make Mario run across the screen
date: 2019-10-05T16:28:59+00:00
author: sukimayou
layout: post
av_cpm_meta:
  - 'a:2:{s:7:"message";s:0:"";s:9:"published";b:0;}'
avopt_banners_inside_post:
  - 'true'
avopt_banners_on_page:
  - 'true'
categories:
  - Uncategorized
---
I&#8217;m fairly certain this is based off of someone else&#8217;s code, so I&#8217;m relieving it to the public domain (or whatever the rights of the original template is). I do not own Mario nor am I affiliated with Nintendo, so since that&#8217;s out of the way here&#8217;s the GIF used:<figure class="wp-block-image">

<img src="/wp-content/uploads/2019/10/oof.gif" alt="" class="wp-image-41" />

<pre class="wp-block-code"><code>import wx
from wx.adv import Animation
from win32api import GetSystemMetrics

end = GetSystemMetrics(0)
offset = int(GetSystemMetrics(1)*0.75)
speed = 30

mariogif = "mario.gif"

class ShapedFrame(wx.Frame):
    def __init__(self):
        self.x = -100
        self.y = offset
        wx.Frame.__init__(self, None, -1, "Shaped Window", pos=wx.Point(self.x, self.y),
                style = wx.FRAME_SHAPED | wx.SIMPLE_BORDER)
        self.hasShape = False
        self.delta = wx.Point(0,0)

        # Load the image
        self.timer = wx.Timer(self)
        anim = Animation(mariogif)
        self.frames = anim.GetFrameCount()
        self.i = 0
        self.timer.Start(milliseconds = 100, oneShot = False)
        self.Bind(wx.EVT_TIMER, self.OnTimer)
        
        
    def OnTimer(self, evt=None):
        anim = Animation(mariogif)
          
        image = anim.GetFrame(self.i)
        self.i += 1
        if self.i >= self.frames:
            self.i = 0
        image.SetMaskColour(255,255,255)
        image.SetMask(True)            
        self.bmp = wx.Bitmap(image)

        self.SetClientSize((self.bmp.GetWidth(), self.bmp.GetHeight()))
        dc = wx.ClientDC(self)
        dc.DrawBitmap(self.bmp, 0,0, True)
        self.SetWindowShape()
        self.x += speed
        if self.x > end:
            self.x = -100
        self.Move(self.x, self.y)

        #self.Bind(wx.EVT_LEFT_DCLICK, self.OnDoubleClick)
        #self.Bind(wx.EVT_LEFT_DOWN, self.OnLeftDown)
        #self.Bind(wx.EVT_LEFT_UP, self.OnLeftUp)
        #self.Bind(wx.EVT_MOTION, self.OnMouseMove)
        self.Bind(wx.EVT_RIGHT_UP, self.OnExit)
        self.Bind(wx.EVT_PAINT, self.OnPaint)
        self.Bind(wx.EVT_WINDOW_CREATE, self.SetWindowShape)
        self.Bind(wx.EVT_ERASE_BACKGROUND, self.OnEraseBackground)
        
    
    def OnEraseBackground(self,evt=None):
        pass        
    def SetWindowShape(self, evt=None):
        r = wx.Region(self.bmp)
        self.hasShape = self.SetShape(r)

    def OnDoubleClick(self, evt):
        if self.hasShape:
            self.SetShape(wx.Region())
            self.hasShape = False
        else:
            self.SetWindowShape()

    def OnPaint(self, evt):
        dc = wx.PaintDC(self)
        dc.DrawBitmap(self.bmp, 0,0, True)

    def OnExit(self, evt):
        self.Close()

    def OnLeftDown(self, evt):
        self.CaptureMouse()
        pos = self.ClientToScreen(evt.GetPosition())
        origin = self.GetPosition()
        self.delta = wx.Point(pos.x - origin.x, pos.y - origin.y)

    def OnMouseMove(self, evt):
        if evt.Dragging() and evt.LeftIsDown():
            pos = self.ClientToScreen(evt.GetPosition())
            newPos = (pos.x - self.delta.x, pos.y - self.delta.y)
            self.Move(newPos)

    def OnLeftUp(self, evt):
        if self.HasCapture():
            self.ReleaseMouse()
            
if __name__ == '__main__':
    app = wx.App()
    ShapedFrame().Show()
    app.MainLoop()  </code></pre>