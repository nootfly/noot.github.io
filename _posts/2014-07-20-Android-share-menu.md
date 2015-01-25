---
layout: post
title:  "Android share menu"
date:   2014-07-20 22:01:30+1000
categories: android
tags: android
---
This blog shows how to create a share menu in an app.

1. Create a menu item in a menu xml file.

           <item android:id="@+id/action_share"
                 app:showAsAction="always"
                 android:title="@string/action_share"
                 app:actionProviderClass=
                 "android.support.v7.widget.ShareActionProvider" />
            
2. Override onCreateOptionsMenu
   
           @Override
        public void onCreateOptionsMenu(Menu menu, MenuInflater inflater) {
            inflater.inflate(R.menu.detailfragment, menu);
            MenuItem menuItem = menu.findItem(R.id.action_share);
            ShareActionProvider mShareActionProvider = (ShareActionProvider)         MenuItemCompat.getActionProvider(menuItem);
            if (mShareActionProvider != null) {
                mShareActionProvider.setShareIntent(createShareForecastIntent());
            } else {
                Log.d(LOG_TAG, "Share Action Provider is null?");
            }
        }

        private Intent createShareForecastIntent() {
            Intent shareIntent = new Intent(Intent.ACTION_SEND);
            shareIntent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_WHEN_TASK_RESET);
            shareIntent.setType("text/plain");
            shareIntent.putExtra(Intent.EXTRA_TEXT, mForecastStr + FORECAST_SHARE_HASHTAG);
            return shareIntent;
        }
        