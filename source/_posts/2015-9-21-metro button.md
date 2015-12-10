title:  An Metro sytle button build with wxWidgets!
date:   2015-09-21 11:23:32
categories: wxWidgets
tags: wxWidgets
---

# An Metro sytle button build with wxwidget

Here's how to implement a metro style button with wxwidgets
  - no-border
  - flat
  - foucsed, pushed, etc. event enable
  - default button event behavior 

```C

class MetroButton: public wxButton {
 public:
  typedef enum {
    BT_STATE_NORMAL,
    BT_STATE_FOCUS,
    BT_STATE_SELECT
  } BT_STATE;

 private:
  wxWindow* m_parent;
  bool have_focus_;    // mouse enter
  bool selected_;      // mouse left button down
  bool highlighted_;   // default selected button
  wxString label_;

  BT_STATE state_;
 public:
  MetroButton(wxWindow* parent, int id, wxString label)
    : wxButton(parent, id, label, wxDefaultPosition, wxSize(90, 30), wxNO_BORDER)
    , have_focus_(false)
    , selected_(false)
    , highlighted_(false)
    , label_(label) {
    m_parent = parent;

    Connect(wxEVT_PAINT, wxPaintEventHandler(MetroButton::OnPaint));
    Connect(wxEVT_ENTER_WINDOW, wxMouseEventHandler(MetroButton::OnEnter));
    Connect(wxEVT_LEAVE_WINDOW, wxMouseEventHandler(MetroButton::OnLeave));
    Connect(wxEVT_LEFT_DOWN, wxMouseEventHandler(MetroButton::OnLeftDown));

    SetBackgroundColour(wxColour(51, 51, 51));

    state_ = BT_STATE_NORMAL;
  }

  void PrepareDCColor(wxPaintDC& dc) {
    const static wxColour color_bk_normal = wxColour(56, 56, 56);
    const static wxColour color_bk_focus = wxColour(28, 28, 28);
    const static wxColour color_bk_selected_ = wxColour(152, 187, 211);

    const static wxColour color_bordline_normal_ = wxColour(56, 56, 56);
    const static wxColour color_bordline_focus = wxColour(56, 56, 56);
    const static wxColour color_bordline_selected_ = wxColour(152, 187, 211);

    const static wxColour color_text_normal = wxColour(192, 192, 192);
    const static wxColour color_text_line_focus = wxColour(192, 192, 192);
    const static wxColour color_text_line_selected_ = wxColour(0, 0, 0);

    switch (state_) {
    case BT_STATE_SELECT:
      dc.SetPen(wxPen(color_bordline_selected_));
      dc.SetBrush(wxBrush(color_bk_selected_));
      dc.SetTextForeground(color_text_line_selected_);
      break;
    case BT_STATE_FOCUS:
      dc.SetPen(wxPen(color_bordline_focus));
      dc.SetBrush(wxBrush(color_bk_focus));
      dc.SetTextForeground(color_text_line_focus);
      break;
    case BT_STATE_NORMAL:
      dc.SetPen(wxPen(color_bordline_normal_));
      dc.SetBrush(wxBrush(color_bk_normal));
      dc.SetTextForeground(color_text_normal);
      break;
    default:
      break;
    }
  }

  void OnPaint(wxPaintEvent& event) {
    wxPaintDC dc(this);
    wxSize size = GetSize();

    PrepareDCColor(dc);
    dc.DrawRectangle(0, 0, this->GetSize().GetWidth(), GetSize().GetHeight());

    wxFont textFont = GetFont();
    dc.SetFont(textFont);
    wxSize label_size = dc.GetTextExtent(label_);
    wxPoint label_pt = wxPoint((size.GetWidth() - label_size.GetWidth()) / 2,
                               (size.GetHeight() - label_size.GetHeight()) / 2);

    dc.DrawText(label_, label_pt);

    event.Skip();
  }

  void OnEnter(wxMouseEvent& event) {
    state_ = MetroButton::BT_STATE_FOCUS;
    have_focus_ = true;
    Refresh(false);
    Update();
    event.Skip();
  }

  void OnLeave(wxMouseEvent& event) {
    state_ = MetroButton::BT_STATE_NORMAL;
    have_focus_ = false;
    selected_ = false;
    Refresh(false);
    Update();
    event.Skip();
  }

  void OnLeftDown(wxMouseEvent& event) {
    state_ = MetroButton::BT_STATE_SELECT;
    selected_ = true;
    Refresh(false);
    Update();
    event.Skip();
  }
};
```

the screen shot looks like:
![normal](http://bianliu1013.github.io/image/2015-9-21-metro%20button1.png)
![hover](http://bianliu1013.github.io/image/2015-9-21-metro%20button2.png)
![pushed](http://bianliu1013.github.io/image/2015-9-21-metro%20button3.png)
