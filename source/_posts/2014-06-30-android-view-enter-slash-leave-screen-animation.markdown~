---
layout: post
title: "Android view enter/leave screen animation"
date: 2014-06-30 15:21:44 +0800
comments: true
categories: [Android, animation]
---

package com.feinno.jiosocial.ui.activitys.avchat;

import android.view.View;
import android.view.ViewGroup;
import android.view.animation.Animation;
import android.view.animation.Transformation;
import android.widget.LinearLayout;
import android.widget.RelativeLayout;
import android.widget.FrameLayout;

public class ExpandAnimation extends Animation {
    private View mView;
    private ViewGroup.LayoutParams mViewLayoutParams;
    private int mMarginStart, mMarginEnd;
    private boolean mIsVisibleAfter = false;
    private boolean mWasEndedAlready = false;
    private boolean mFromBottom = false;

    /**
     * Initialize the animation
     * @param view The layout we want to animate
     * @param duration The duration of the animation, in ms
     * @param fromBottom The layout is at the bottom or top of screen
     */
    public ExpandAnimation(View view, int duration, boolean fromBottom) {
        setDuration(duration);
        mView = view;
        
        mViewLayoutParams = mView.getLayoutParams();
        int bMargin = 0;
        int tMargin = 0;
        if(mViewLayoutParams instanceof LinearLayout.LayoutParams) {
        	bMargin = ((LinearLayout.LayoutParams)mViewLayoutParams).bottomMargin;
        	tMargin = ((LinearLayout.LayoutParams)mViewLayoutParams).topMargin;
        } else if(mViewLayoutParams instanceof RelativeLayout.LayoutParams) {
        	bMargin = ((RelativeLayout.LayoutParams)mViewLayoutParams).bottomMargin;
        	tMargin = ((RelativeLayout.LayoutParams)mViewLayoutParams).topMargin;
        } else if(mViewLayoutParams instanceof FrameLayout.LayoutParams) {
        	bMargin = ((FrameLayout.LayoutParams)mViewLayoutParams).bottomMargin;
        	tMargin = ((FrameLayout.LayoutParams)mViewLayoutParams).topMargin;
        }
        // decide to show or hide the view
//        mIsVisibleAfter = (view.getVisibility() == View.VISIBLE);

        mFromBottom = fromBottom;
        if(mFromBottom) {
	        mMarginStart = bMargin;	        
        } else {
        	mMarginStart = tMargin;
        }
        
        if(mMarginStart == 0) {
        	mMarginEnd = (0 - mView.getHeight()) - 1;
        	mIsVisibleAfter = true;
        } else {
        	mMarginEnd = 0;
        	mIsVisibleAfter = false;
        }
        mView.setVisibility(View.VISIBLE);
    }

<!-- more -->

    @Override
    protected void applyTransformation(float interpolatedTime, Transformation t) {
        super.applyTransformation(interpolatedTime, t);

        int margin = 0;
        boolean needUpdate = false;
        if (interpolatedTime < 1.0f) {
            // Calculating the new bottom margin
        	margin = mMarginStart
                    + (int) ((mMarginEnd - mMarginStart) * interpolatedTime);
        	needUpdate = true;
        } else if(!mWasEndedAlready) {
        	margin = mMarginEnd;
        	needUpdate = true;
        }
        
        if(needUpdate) {
	        if(mViewLayoutParams instanceof LinearLayout.LayoutParams) {
	        	if(mFromBottom) {
	        		((LinearLayout.LayoutParams)mViewLayoutParams).bottomMargin = margin;
	        	} else {
	        		((LinearLayout.LayoutParams)mViewLayoutParams).topMargin = margin;
	        	}
	        	
	        } else if(mViewLayoutParams instanceof RelativeLayout.LayoutParams) {            	
	        	if(mFromBottom) {
	        		((RelativeLayout.LayoutParams)mViewLayoutParams).bottomMargin = margin;
	        	} else {
	        		((RelativeLayout.LayoutParams)mViewLayoutParams).topMargin = margin;
	        	}
	        	
	        } else if(mViewLayoutParams instanceof FrameLayout.LayoutParams) {            	
	        	if(mFromBottom) {
	        		((FrameLayout.LayoutParams)mViewLayoutParams).bottomMargin = margin;
	        	} else {
	        		((FrameLayout.LayoutParams)mViewLayoutParams).topMargin = margin;
	        	}        	
	        }

	    	mView.requestLayout();
        }

    	if (interpolatedTime < 1.0f) {
    		
    	}else if (!mWasEndedAlready) {
            if (mIsVisibleAfter) {
            	mView.setVisibility(View.GONE);
            }
            mWasEndedAlready = true;
        }
    }
}

